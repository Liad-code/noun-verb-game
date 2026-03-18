---
stepsCompleted: [1, 2, 3, 4, 5, 6, 7, 8]
lastStep: 8
status: 'complete'
completedAt: '2026-03-14'
inputDocuments:
  - _bmad-output/planning-artifacts/product-brief-TRIPO-2026-03-12.md
  - _bmad-output/planning-artifacts/prd.md
  - _bmad-output/planning-artifacts/ux-design-specification.md
workflowType: 'architecture'
project_name: 'TRIPO'
user_name: 'Liad'
date: '2026-03-14'
---

# Architecture Decision Document

**Author:** Liad
**Date:** 2026-03-14

_This document builds collaboratively through step-by-step discovery. Sections are appended as we work through each architectural decision together._

---

## Project Context Analysis

### Requirements Overview

**Functional Requirements (27 total):**

- **Conversational Interface (FR1–FR7):** Hebrew NLP input, constraint parsing, streaming chat UI, Context Confirmation Card, affiliate CTAs, zero-result fallback. Architectural implication: LLM at the center of every search request, SSE/WebSocket streaming from server to client.

- **Search & Aggregation (FR8–FR14):** Parallel Kiwi + Booking.com calls (TLV-origin only), max 3 curated packages, ILS conversion, 24h exchange rate refresh, 15-min result cache, 3-call concurrency cap per provider. Architectural implication: Server-side orchestration layer with parallel fetch, queue/rate-limit middleware, caching layer.

- **Context & Memory (FR15–FR18):** Full constraint persistence across session turns, selective overwrite, 30-day auto-purge of conversation logs. Architectural implication: Two-tier storage — fast ephemeral session context (anonymous) + persistent DB (authenticated). Scheduled purge job required.

- **User Accounts (FR19–FR23):** Anonymous browsing, save-triggered sign-up, Israeli phone OTP auth, Saved Trips Dashboard, re-initiate from saved context. Architectural implication: SMS provider hard dependency, JWT session tokens, user + trip DB tables.

- **Operations (FR24–FR27):** Hebrew admin dashboard, API latency metrics, affiliate CTR tracking, affiliate ID appended to all outbound URLs. Architectural implication: Admin-scoped routes, instrumentation on every API call and every affiliate redirect, aggregate metrics storage.

**Non-Functional Requirements:**

- 5s end-to-end response (AI + 2 parallel API calls) — forces parallel execution, pre-warmed functions, and aggressive caching
- 60FPS during streaming — React rendering must not block on state updates during AI stream
- TTI < 2.5s on 4G — code splitting, minimal initial bundle, SSR for landing page only
- 100 concurrent sessions — achievable on Vercel serverless without custom infra
- 4.5s API timeout threshold with graceful fallback on every external call
- ±2% ILS accuracy — exchange rate must be refreshed within 24h max staleness

**Scale & Complexity:**

- Primary domain: Full-stack web (Next.js), AI integration, API aggregation
- Complexity level: Medium-High
- Estimated architectural components: 8 major areas (Chat/AI layer, Search orchestration, Context/session store, Auth, Database, Admin, Currency service, Affiliate tracking)

### Technical Constraints & Dependencies

- Framework: Next.js (React) — decided
- Styling: Tailwind CSS + shadcn/ui — decided
- Deployment target: Vercel (inferred from Next.js + solo founder + bootstrap budget)
- External APIs: Kiwi (flights), Booking.com (hotels) — hard business dependencies
- SMS provider: Required for phone OTP (Twilio Verify or equivalent)
- LLM: Required for Hebrew NLP + constraint extraction + response generation
- Bootstrap budget: Free/cheap tiers must be prioritized at every infrastructure choice
- Solo developer: Operational complexity must be minimized — managed services preferred

### Cross-Cutting Concerns Identified

1. **Hebrew/RTL** — affects every UI component, all copy, all ARIA labels
2. **Error handling & graceful fallback** — required on every external call (4.5s timeout)
3. **Affiliate ID injection** — every outbound Kiwi and Booking.com URL
4. **ILS currency conversion** — every price displayed anywhere in the UI
5. **Auth state (anonymous vs. authenticated)** — affects save behavior and context persistence throughout the entire app
6. **Rate limiting** — protects Kiwi + Booking.com API quotas (FR14)
7. **Data retention compliance** — 30-day purge, Israeli Privacy Protection Law
8. **Affiliate disclosure** — legally required on every booking CTA

---

## Starter Template Evaluation

### Primary Technology Domain

Full-stack web application — Next.js 15 App Router with integrated API routes, serving both the React SPA frontend and the backend API layer from a single codebase.

### Selected Starter: create-next-app + shadcn/ui

**Rationale:** The PRD and UX spec explicitly specify this stack. No alternatives evaluated — the stack is already decided and is the industry standard for Next.js projects in 2026. create-next-app provides all foundational tooling; shadcn/ui layered on top provides the component system.

**Initialization Commands:**

```bash
# Step 1 — scaffold the project
npx create-next-app@latest tripo \
  --typescript \
  --tailwind \
  --app \
  --src-dir \
  --turbopack

# Step 2 — initialize shadcn/ui component system
npx shadcn@latest init
```

**Architectural Decisions Provided by Starter:**

**Language & Runtime:**
- TypeScript strict mode throughout — no plain JS files
- Node.js runtime via Next.js (serverless functions on Vercel)

**Styling Solution:**
- Tailwind CSS v4 — utility-first, RTL variant support via `rtl:` prefix
- shadcn/ui components copy-owned in `src/components/ui/` — fully customizable, no upstream version conflicts
- CSS variables in `src/app/globals.css` for design tokens (as defined in UX spec Step 8)

**Build Tooling:**
- Turbopack for development (fast HMR)
- Next.js production build for deployment
- Import alias `@/*` → `src/*` — clean imports throughout

**Testing Framework:**
- Vitest + React Testing Library (to be added — lightweight, Turbopack-compatible)

**Code Organization:**
```
src/
├── app/
│   ├── (chat)/             # Main chat interface route group
│   ├── (auth)/             # Auth routes (OTP flow)
│   ├── admin/              # Admin dashboard (protected)
│   ├── api/                # API routes (AI stream, search, auth, admin)
│   ├── layout.tsx          # Root layout — dir="rtl", Rubik font
│   └── globals.css         # Design tokens (:root CSS variables)
├── components/
│   ├── ui/                 # shadcn/ui base components (copy-owned)
│   ├── chat/               # Chat-specific components
│   ├── cards/              # TripPackageCard, ContextConfirmationCard
│   └── shared/             # Reusable across features
├── lib/                    # Utilities, API clients, helpers
├── hooks/                  # Custom React hooks
└── types/                  # TypeScript interfaces
```

**Development Experience:**
- Turbopack dev server: `npm run dev`
- `@/*` import aliases throughout
- ESLint pre-configured
- `dir="rtl"` set on `<html>` in `src/app/layout.tsx` on day one

**Note:** Project initialization is the first implementation story.

---

## Core Architectural Decisions

### Decision Priority Analysis

**Critical (block implementation):**
- Database: Neon PostgreSQL + Drizzle ORM
- Caching + session store: Upstash Redis
- AI streaming: SSE via Next.js Route Handlers (replaces PRD WebSocket spec — SSE is one-directional, fully Vercel-compatible, simpler)
- LLM: Claude API `claude-sonnet-4-6` via `@anthropic-ai/sdk`
- Auth: Twilio Verify (SMS OTP) + JWT in httpOnly cookie

**Important (shape architecture):**
- State management: Zustand (client) + TanStack Query (server)
- Admin auth: Single `ADMIN_SECRET` env var checked in Next.js middleware on `/admin/*`
- Scheduled jobs: Vercel Cron (2 jobs — exchange rate + 30-day purge)

**Deferred (post-MVP):**
- Full APM/observability (Sentry, Datadog)
- CDN for destination images
- Message queue for API orchestration at scale

### Data Architecture

**Database:** Neon (PostgreSQL serverless) — free tier (0.5GB), Vercel native integration, auto-pauses when idle
**ORM:** Drizzle ORM — TypeScript-native, minimal cold-start overhead, excellent Neon integration. Prisma excluded due to query engine cold-start latency on serverless.
**Caching:** Upstash Redis — free tier (10k req/day). Handles: API result cache (15-min TTL), ILS exchange rate (24h TTL), anonymous session context (30-day TTL)
**Anonymous session context:** Stored in Upstash Redis keyed by `session:{uuid}`; session ID set as httpOnly cookie (`tripo_sid`) on first message send

**Core DB schema (tables):**
- `users` — id, phone_number, created_at
- `sessions` — id, user_id, jwt_token_hash, expires_at
- `saved_trips` — id, user_id, trip_data (JSONB), context_snapshot (JSONB), created_at
- `affiliate_clicks` — id, trip_id, provider, clicked_at
- `api_metrics` — id, provider, latency_ms, status, recorded_at

### Authentication & Security

**SMS OTP:** Twilio Verify — Israeli +972 coverage, $0.05/verification, simple Node SDK
**Session tokens:** JWT (30-day expiry) in httpOnly SameSite=Strict cookie — XSS-safe, no localStorage
**Admin access:** `ADMIN_SECRET` env var validated by Next.js middleware on `/admin/*` — no user-facing admin signup for MVP
**API security:** Zod validation on all API route inputs; Vercel edge middleware rate-limiting per IP

### API & Communication Patterns

**AI streaming:** SSE via Next.js Route Handlers (`ReadableStream`, `Content-Type: text/event-stream`). PRD specified WebSocket — revised because Tripo's stream is one-directional and SSE runs natively on Vercel serverless functions without extra infrastructure.

**LLM:** `@anthropic-ai/sdk`, model `claude-sonnet-4-6`. Streaming SDK output piped directly into SSE `ReadableStream`.

**API routes:**
- `POST /api/chat` — user message + session ID → LLM + parallel Kiwi/Booking.com → SSE stream
- `POST /api/auth/send-otp` — Twilio Verify trigger
- `POST /api/auth/verify-otp` — verify code, set JWT cookie
- `GET  /api/trips` — saved trips (authenticated)
- `POST /api/trips` — save trip (authenticated)
- `POST /api/affiliate-click` — record click event for CTR tracking
- `GET  /api/admin/metrics` — latency + CTR data (admin JWT)
- `POST /api/cron/exchange-rate` — called by Vercel Cron daily (internal)
- `POST /api/cron/purge-sessions` — called by Vercel Cron daily (internal)

**Error handling:** All routes return `{ error: string, code: string }` on failure. External API calls enforce 4.5s timeout; on timeout, SSE sends a Hebrew fallback message before stream closes.

**Concurrency:** `p-limit` enforces max 3 concurrent calls per provider (FR14). Kiwi + Booking.com calls run via `Promise.all` within that limit.

### Frontend Architecture

**Client state — Zustand:** Chat message list, streaming text buffer, context chip state, auth state (user / isAnonymous), UI state (bottom sheet open/closed)
**Server state — TanStack Query:** Saved trips, admin metrics — automatic caching, background refetch, loading/error states
**`useChat` hook:** Single owner of SSE connection lifecycle, chunk accumulation, context chip extraction, results dispatch. All chat state flows through this hook.

**2-Tab layout:** The main interface is split into two tabs:
- Tab 1 "שיחה" (Chat): Conversation, status messages, chips, AI notification messages only. ❌ No prices, no CTAs, no cards.
- Tab 2 "הטיול שלי" (My Trip): All `TripPackageCard` results, affiliate CTAs, prices. Empty state shown until search results arrive.

**Routing:**
- `/` → chat interface (main experience, anonymous-first)
- `/trips` → Saved Trips Dashboard (auth required; redirects to `/` if anonymous)
- `/admin` → Admin dashboard (admin JWT required)

### Infrastructure & Deployment

**Hosting:** Vercel free tier — serverless functions, edge middleware, zero-config Next.js deployment
**CI/CD:** GitHub → Vercel auto-deploy on push to `main`; preview deployments on all branches
**Scheduled jobs (Vercel Cron — 2 jobs, within free tier limit):**
- `0 3 * * *` daily → `/api/cron/exchange-rate` — refreshes ILS rate in Redis
- `0 2 * * *` daily → `/api/cron/purge-sessions` — deletes records older than 30 days

**Environment variables:**
`ANTHROPIC_API_KEY`, `KIWI_API_KEY`, `BOOKING_API_KEY`, `TWILIO_ACCOUNT_SID`, `TWILIO_AUTH_TOKEN`, `TWILIO_VERIFY_SID`, `DATABASE_URL`, `UPSTASH_REDIS_URL`, `UPSTASH_REDIS_TOKEN`, `JWT_SECRET`, `ADMIN_SECRET`, `EXCHANGE_RATE_API_KEY`

### Decision Impact Analysis

**Implementation sequence:**
1. Project init + DB schema + Drizzle migration
2. Redis connection + session middleware
3. Auth flow (Twilio OTP → JWT cookie)
4. `POST /api/chat` SSE route (LLM only, no external APIs)
5. Chat UI (`useChat` hook + streaming rendering)
6. Kiwi + Booking.com integration into `/api/chat`
7. Trip saving + Saved Trips Dashboard
8. Admin dashboard + affiliate click tracking
9. Vercel Cron jobs (exchange rate + purge)

**Cross-component dependencies:**
- Every price display depends on the exchange rate in Redis
- Every affiliate CTA depends on the affiliate ID injection utility
- `useChat` is the single source of truth for all chat state
- Admin metrics depend on instrumentation added in steps 4 and 6

---

## Implementation Patterns & Consistency Rules

### Critical Conflict Points Identified

9 areas where AI developer agents could make incompatible choices without explicit rules: naming conventions, API response shape, SSE event protocol, RTL/Hebrew handling, price formatting, affiliate URL construction, error message language, state ownership, and test co-location.

### Naming Patterns

**Database (Drizzle schema + Neon PostgreSQL):**
- Tables: `snake_case` plural — `users`, `saved_trips`, `affiliate_clicks`, `api_metrics`
- Columns: `snake_case` — `user_id`, `created_at`, `trip_data`, `latency_ms`
- Foreign keys: `{referenced_table_singular}_id` — `user_id`, `trip_id`
- Indexes: `idx_{table}_{column}` — `idx_users_phone_number`
- ✅ `saved_trips`, `user_id`, `created_at` &nbsp;&nbsp; ❌ `SavedTrips`, `userId`, `createdAt`

**API Routes:**
- Paths: `kebab-case`, plural nouns — `/api/trips`, `/api/affiliate-click`
- Cron routes: `/api/cron/{job-name}` — `/api/cron/exchange-rate`
- No verbs in paths — HTTP method conveys the action
- ✅ `POST /api/trips` &nbsp;&nbsp; ❌ `POST /api/saveTrip`

**TypeScript / React:**
- Components: `PascalCase` — `TripPackageCard`, `ChatPage`
- Hooks: `useCamelCase` — `useChat`, `useAuth`
- Utilities: `camelCase` — `formatILS`, `buildAffiliateUrl`
- Zustand stores: `useCamelCaseStore` — `useChatStore`, `useAuthStore`
- Types/interfaces: `PascalCase` — `TripPackageCardProps`, `FlightLeg`
- Booleans: `is`/`has`/`can` prefix — `isLoading`, `isAuthenticated`, `hasResults`

**Files:**
- React components: `PascalCase.tsx` — `TripPackageCard.tsx`
- Hooks: `useCamelCase.ts` — `useChat.ts`
- Next.js App Router files: `route.ts`, `page.tsx`, `layout.tsx` (framework convention)
- Utilities: `camelCase.ts` — `formatILS.ts`, `buildAffiliateUrl.ts`
- Tests: co-located — `TripPackageCard.test.tsx` next to `TripPackageCard.tsx`

### Structure Patterns

**Component organization:**
```
src/components/
├── ui/       # shadcn/ui primitives ONLY (Button, Card, Badge, Sheet)
├── chat/     # Chat-domain (ChatPage, MessageBubble, TypingIndicator, InputBar)
├── cards/    # Result cards (TripPackageCard, ContextConfirmationCard, SkeletonCard)
└── shared/   # Cross-domain (PriceDisplay, AffiliateButton, LoadingSpinner)
```
`ui/` contains ONLY shadcn/ui components — never custom business logic.

**Lib organization:**
```
src/lib/
├── db/       # Drizzle client, schema.ts, migrations
├── redis/    # Upstash client, session helpers
├── ai/       # Anthropic client, system prompt, constraint extraction
├── search/   # Kiwi client, Booking.com client, aggregator
├── auth/     # JWT helpers, Twilio helpers
└── utils/    # formatILS, buildAffiliateUrl, extractConstraints
```

**Tests:** Co-located with source — `Component.test.tsx` alongside `Component.tsx`. ❌ No top-level `__tests__/` directory.

### Format Patterns

**API success response:** `{ data: T }` — always wrapped.

**API error response:** `{ error: string, code: string }` — always this exact shape. ❌ Never `{ success: false }` or `{ status: "error" }`.

**SSE Event Protocol — critical for chat stream consistency:**
```typescript
type SSEEvent =
  | { type: 'status';  payload: { text: string } }                     // "מחפש טיסות..."
  | { type: 'text';    payload: { chunk: string } }                    // AI text chunk
  | { type: 'chips';   payload: { chips: ConfirmationChip[] } }        // Context Confirmation Card
  | { type: 'results'; payload: { packages: TripPackage[] } }          // Trip tab results batch
  | { type: 'done';    payload: null }                                  // stream complete
  | { type: 'error';   payload: { message: string } }                  // stream-level error
// Wire format: "data: {JSON}\n\n"
```
❌ Never send raw text without a type wrapper. ❌ Never invent new event types without updating this spec and `useChat`.

**`results` event semantics:** Carries all trip packages as a single batch (up to 3). The client stores them in `tripPackages` (chatStore) and sets `tripTabHasNewResults = true` (uiStore), then navigates the user to the Trip tab. The AI simultaneously sends a Hebrew notification message via the regular `text`/`done` events: "מצאתי אפשרויות! עבור ללשונית הטיול לראות את התוצאות ✈️".

**Dates in API:** ISO 8601 — `"2026-07-24T06:15:00Z"`. DB: `TIMESTAMP WITH TIME ZONE`.

**JSON field naming:** `camelCase` in API payloads — `{ tripId, totalPriceILS, createdAt }`. snake_case stays in DB only.

### Communication Patterns

**Zustand store ownership:**
- `useChatStore`: `messages`, `streamingText`, `contextChips`, `tripPackages`, `isStreaming`, `sessionId`, `statusMessages`, `showTypingIndicator`, `updatingChipType`
- `useAuthStore`: `user`, `isAuthenticated`, `isAnonymous`
- `useUIStore`: `isSaveSheetOpen`, `expandedCardId`, `activeTab`, `tripTabHasNewResults`

All Zustand updates are immutable — `set(state => ({ ...state, newValue }))`. ❌ No `useState` for data belonging in a store.

**TanStack Query key factory (all agents use this — no ad-hoc strings):**
```typescript
const queryKeys = {
  trips: () => ['trips'] as const,
  trip: (id: string) => ['trips', id] as const,
  adminMetrics: () => ['admin', 'metrics'] as const,
}
```

### Process Patterns

**Error language rule (critical):**
- User-facing messages: **Hebrew always** — `"לא הצלחנו למצוא טיסות"`
- Server logs: **English always** — `console.error('[api/chat] Kiwi timeout')`
- ❌ Never log Hebrew; ❌ Never show English to users

**All API route handlers have a top-level try/catch.** No unhandled promise rejections.

**Loading state naming:** `isLoading` (initial fetch), `isStreaming` (SSE active), `isPending` (mutation), `isFetching` (background refetch). ❌ Never bare `loading` or `fetching`.

**Hebrew/RTL:** `dir="rtl"` set **once** on `<html>` in `src/app/layout.tsx`. ❌ Never on any component below `<html>`.

**Price display:** Always `formatILS(amount, sourceCurrency?, sourceAmount?)`. ❌ Never direct `Intl.NumberFormat` in components.

**Affiliate URLs:** Always `buildAffiliateUrl(url, provider)`. ❌ Never manual affiliate ID appending.

### Enforcement Summary — All AI Agents MUST:

1. Follow the SSE event protocol exactly — no undocumented event types
2. Use `formatILS()` for every price — zero direct `Intl.NumberFormat` in components
3. Use `buildAffiliateUrl()` for every outbound booking link
4. Write user-facing errors in Hebrew, server logs in English
5. Co-locate tests — no `__tests__/` directories
6. Use the `queryKeys` factory for all TanStack Query keys
7. Keep `useChatStore` as the sole owner of all chat state
8. Never set `dir="rtl"` below `<html>`

---

## Project Structure & Boundaries

### Complete Project Directory Structure

```
tripo/
├── .github/
│   └── workflows/
│       └── ci.yml                      # Lint + typecheck on PR
├── public/
│   └── fonts/                          # Rubik font fallback
├── src/
│   ├── app/                            # Next.js App Router
│   │   ├── layout.tsx                  # Root: dir="rtl", Rubik font, metadata
│   │   ├── globals.css                 # :root design tokens, @keyframes, base styles
│   │   ├── page.tsx                    # Redirects → /(chat)
│   │   ├── (chat)/                     # Route group — main chat interface
│   │   │   ├── layout.tsx              # Chat shell (header + input bar frame)
│   │   │   └── page.tsx                # ChatPage
│   │   ├── trips/
│   │   │   └── page.tsx                # Saved Trips Dashboard (auth-gated)
│   │   ├── admin/
│   │   │   └── page.tsx                # Admin dashboard (ADMIN_SECRET protected)
│   │   └── api/
│   │       ├── chat/
│   │       │   └── route.ts            # POST → LLM + search → SSE stream (FR1–FR16)
│   │       ├── auth/
│   │       │   ├── send-otp/route.ts   # POST → Twilio Verify (FR21)
│   │       │   └── verify-otp/route.ts # POST → JWT cookie (FR21)
│   │       ├── trips/
│   │       │   └── route.ts            # GET (FR22) / POST (FR20)
│   │       ├── affiliate-click/
│   │       │   └── route.ts            # POST → CTR event (FR27)
│   │       ├── admin/
│   │       │   └── metrics/route.ts    # GET → latency + CTR (FR24–FR26)
│   │       └── cron/
│   │           ├── exchange-rate/route.ts   # POST → refresh ILS in Redis (FR12)
│   │           └── purge-sessions/route.ts  # POST → delete >30 day records (FR18)
│   │
│   ├── components/
│   │   ├── ui/                         # shadcn/ui primitives only
│   │   │   ├── button.tsx
│   │   │   ├── card.tsx
│   │   │   ├── badge.tsx
│   │   │   ├── sheet.tsx               # Bottom sheet (sign-up, chip edit)
│   │   │   ├── input.tsx
│   │   │   └── textarea.tsx
│   │   ├── chat/                       # Chat tab components (FR1–FR7)
│   │   │   ├── ChatPage.tsx            # 2-tab layout root (ChatHeader+TabBar+panels)
│   │   │   ├── ChatHeader.tsx
│   │   │   ├── TabBar.tsx              # "שיחה" / "הטיול שלי" tab navigation
│   │   │   ├── ChatStream.tsx
│   │   │   ├── MessageBubble.tsx
│   │   │   ├── TypingIndicator.tsx
│   │   │   ├── StatusMessage.tsx       # "מחפש טיסות..."
│   │   │   ├── WarmColdStart.tsx       # Greeting + suggestion chips
│   │   │   ├── SuggestionChips.tsx
│   │   │   ├── InputBar.tsx
│   │   │   └── ScrollToBottomChip.tsx
│   │   ├── trip/                       # Trip tab components (FR3–FR4, FR10)
│   │   │   └── TripTab.tsx             # Trip results panel + empty state
│   │   ├── cards/                      # Shared card components (FR5–FR6)
│   │   │   ├── TripPackageCard.tsx
│   │   │   ├── TripPackageCard.test.tsx
│   │   │   ├── ContextConfirmationCard.tsx
│   │   │   ├── ContextConfirmationCard.test.tsx
│   │   │   ├── ConfirmationChip.tsx
│   │   │   └── SkeletonCard.tsx
│   │   ├── auth/                       # Auth flow (FR19–FR21)
│   │   │   ├── SaveTripSheet.tsx       # Bottom sheet: phone → OTP → save
│   │   │   └── OTPInput.tsx
│   │   └── shared/
│   │       ├── PriceDisplay.tsx        # Wraps formatILS
│   │       ├── AffiliateButton.tsx     # CTA wrapping buildAffiliateUrl
│   │       └── AffiliateDisclosure.tsx # Legal disclosure (FR27)
│   │
│   ├── hooks/
│   │   ├── useChat.ts                  # SSE lifecycle, all chat state (FR1–FR6)
│   │   ├── useAuth.ts                  # Auth state, OTP flow (FR19–FR21)
│   │   └── useSavedTrips.ts            # TanStack Query for /api/trips (FR22–FR23)
│   │
│   ├── stores/
│   │   ├── chatStore.ts                # Zustand: messages, chips, cards, streaming
│   │   ├── authStore.ts                # Zustand: user, isAuthenticated, isAnonymous
│   │   └── uiStore.ts                  # Zustand: isSaveSheetOpen, expandedCardId
│   │
│   ├── lib/
│   │   ├── db/
│   │   │   ├── client.ts               # Drizzle + Neon connection
│   │   │   ├── schema.ts               # users, saved_trips, affiliate_clicks, api_metrics
│   │   │   └── migrations/
│   │   ├── redis/
│   │   │   ├── client.ts               # Upstash Redis client
│   │   │   └── session.ts              # Session CRUD (get/set/delete context)
│   │   ├── ai/
│   │   │   ├── client.ts               # Anthropic SDK (claude-sonnet-4-6)
│   │   │   ├── systemPrompt.ts         # Hebrew system prompt + constraint rules
│   │   │   └── extractConstraints.ts   # AI output → ConfirmationChip[]
│   │   ├── search/
│   │   │   ├── kiwi.ts                 # Kiwi API client (FR8, FR13, FR14)
│   │   │   ├── booking.ts              # Booking.com client (FR9, FR13, FR14)
│   │   │   └── aggregator.ts           # Parallel fetch → max 3 packages (FR10)
│   │   ├── auth/
│   │   │   ├── jwt.ts                  # Sign/verify JWT, cookie helpers
│   │   │   └── twilio.ts               # Twilio Verify send/check
│   │   └── utils/
│   │       ├── formatILS.ts            # ILS formatter — only price formatter (FR11)
│   │       ├── buildAffiliateUrl.ts    # Affiliate ID injection (FR27)
│   │       └── rateLimit.ts            # p-limit wrapper, max 3/provider (FR14)
│   │
│   ├── types/
│   │   ├── trip.ts                     # TripPackage, FlightLeg, HotelResult, TripPackageCardProps
│   │   ├── chat.ts                     # SSEEvent, ConfirmationChip, ChipType, Message
│   │   ├── auth.ts                     # User, Session, JWTPayload
│   │   └── api.ts                      # API request/response types
│   │
│   └── middleware.ts                   # Admin route protection + rate limiting
│
├── drizzle.config.ts                   # Drizzle Kit migrations config
├── next.config.ts                      # Next.js config (cron headers, env)
├── tailwind.config.ts                  # Design tokens (UX spec Step 8)
├── tsconfig.json
├── package.json
├── .env.local                          # Local secrets (gitignored)
├── .env.example                        # All required vars documented
└── vercel.json                         # Cron job schedules
```

### Architectural Boundaries

**API route authorization:**

| Route | Auth | Rate limited | Streams |
|---|---|---|---|
| `POST /api/chat` | Session cookie | Per IP | SSE |
| `POST /api/auth/send-otp` | None | Per phone | No |
| `POST /api/auth/verify-otp` | None | Per phone | No |
| `GET /api/trips` | JWT cookie | No | No |
| `POST /api/trips` | JWT cookie | No | No |
| `POST /api/affiliate-click` | None | No | No |
| `GET /api/admin/metrics` | ADMIN_SECRET | No | No |
| `POST /api/cron/*` | Vercel Cron header | No | No |

**Component responsibilities:**
- `ChatPage` — composes 2-tab layout: ChatHeader + TabBar + conditional ChatStream/TripTab + InputBar (chat tab only). Reads `activeTab` from `useUIStore`. Owns no data state.
- `TabBar` — tab navigation ("שיחה" / "הטיול שלי"). Reads `activeTab` + `tripTabHasNewResults` from `useUIStore`. Calls `setActiveTab`. Shows badge when Trip tab has new results.
- `ChatStream` — renders the chat conversation panel (messages, status, chips). Shown only in "שיחה" tab.
- `TripTab` — renders trip results panel. Shown only in "הטיול שלי" tab. Empty state when `tripPackages` is empty. Renders `TripPackageCard` list when packages arrive (Story 2.8).
- `TripPackageCard` — pure display. Props in, callbacks out. No store access.
- `ContextConfirmationCard` — reads chip props. Calls `onChipEdit`. No search logic.
- `InputBar` — owns textarea value locally. Calls `onSend(text)`. Does not touch chat store. Rendered only when `activeTab === 'chat'`.
- `useChat` — sole owner of SSE connection and `useChatStore` writes.

**Data flow — user message → results:**
```
InputBar.onSend()
  → useChat.sendMessage()
    → POST /api/chat { message, sessionId }
      → Redis: load session context
      → Anthropic: stream + extract constraints
        → SSE { type: 'status' } × 3
        → Promise.all: kiwi.search() + booking.search()
        → aggregator.buildPackages() → max 3 packages
        → SSE { type: 'chips' }
        → SSE { type: 'results', payload: { packages: TripPackage[] } }
        → SSE { type: 'text' } — Hebrew notification message
        → SSE { type: 'done' }
      → Redis: save updated context
    → useChat reads stream:
        'results' → chatStore.setTripPackages() + uiStore.setTripTabHasNewResults(true)
        'text'/'done' → chatStore.messages (notification committed)
      → ChatStream renders notification message in chat tab
      → TripTab renders packages in trip tab (user switches tab)
```

**2-Tab UI Layout:**
```
ChatPage
├── ChatHeader          (always visible)
├── TabBar              (always visible — "שיחה" | "הטיול שלי" + new-results badge)
├── ChatStream          (visible only when activeTab === 'chat')
├── TripTab             (visible only when activeTab === 'trip')
│   ├── [empty state]   "ספר לי על הטיול שלך בלשונית השיחה ואני אמצא לך את הכי טוב 🧳"
│   └── TripPackageCard × 1–3  (rendered by Story 2.8)
└── InputBar            (visible only when activeTab === 'chat')
```

**Tab transition rule:** When a `results` SSE event arrives, `tripTabHasNewResults` is set to `true` and a badge appears on the "הטיול שלי" tab. The user switches tabs manually. Switching to the Trip tab clears the badge (`tripTabHasNewResults = false`).

**Separation rule:** No prices, CTAs, or search result cards ever appear in the Chat tab. The Chat tab contains only: messages, status lines, chips, and the AI notification text. All commercial content lives exclusively in the Trip tab.

### Requirements to Structure Mapping

| FR | Primary file(s) |
|---|---|
| FR1–FR2 (Hebrew NLP) | `lib/ai/client.ts`, `systemPrompt.ts`, `extractConstraints.ts` |
| FR3 (cards in stream) | `components/cards/TripPackageCard.tsx`, `hooks/useChat.ts` |
| FR4 (affiliate CTAs) | `components/shared/AffiliateButton.tsx`, `lib/utils/buildAffiliateUrl.ts` |
| FR5–FR6 (Context Confirmation Card) | `components/cards/ContextConfirmationCard.tsx`, `ConfirmationChip.tsx` |
| FR7 (zero-result fallback) | `lib/search/aggregator.ts` + SSE `error` event in `api/chat/route.ts` |
| FR8 (Kiwi flights) | `lib/search/kiwi.ts` |
| FR9 (Booking.com hotels) | `lib/search/booking.ts` |
| FR10 (max 3 packages) | `lib/search/aggregator.ts` |
| FR11 (ILS conversion) | `lib/utils/formatILS.ts`, `components/shared/PriceDisplay.tsx` |
| FR12 (24h exchange rate) | `api/cron/exchange-rate/route.ts` |
| FR13 (15-min cache) | Redis cache wrapper in `kiwi.ts` + `booking.ts` |
| FR14 (concurrency cap) | `lib/utils/rateLimit.ts` |
| FR15–FR17 (constraint memory) | `lib/redis/session.ts`, `lib/ai/extractConstraints.ts` |
| FR18 (30-day purge) | `api/cron/purge-sessions/route.ts` |
| FR19 (anonymous browsing) | `middleware.ts` (no auth on `/` or `/api/chat`) |
| FR20 (save-triggered sign-up) | `components/auth/SaveTripSheet.tsx` |
| FR21 (phone OTP) | `lib/auth/twilio.ts`, `api/auth/send-otp/`, `api/auth/verify-otp/` |
| FR22 (Saved Trips Dashboard) | `app/trips/page.tsx`, `hooks/useSavedTrips.ts` |
| FR23 (re-initiate from saved) | `hooks/useChat.ts` (accepts initial context param) |
| FR24–FR26 (admin dashboard) | `app/admin/page.tsx`, `api/admin/metrics/route.ts` |
| FR27 (affiliate tracking) | `lib/utils/buildAffiliateUrl.ts`, `api/affiliate-click/route.ts` |

### External Integration Points

| Service | Client | Auth | Notes |
|---|---|---|---|
| Anthropic Claude | `lib/ai/client.ts` | `ANTHROPIC_API_KEY` | Streaming SDK |
| Kiwi | `lib/search/kiwi.ts` | `KIWI_API_KEY` | 4.5s timeout, p-limit |
| Booking.com | `lib/search/booking.ts` | `BOOKING_API_KEY` | 4.5s timeout, p-limit |
| Twilio Verify | `lib/auth/twilio.ts` | SID + token | Israel +972 |
| Neon PostgreSQL | `lib/db/client.ts` | `DATABASE_URL` | Drizzle ORM |
| Upstash Redis | `lib/redis/client.ts` | URL + token | Cache + sessions |
| Exchange Rate API | `api/cron/exchange-rate/route.ts` | `EXCHANGE_RATE_API_KEY` | Daily cron |

---

## Architecture Validation Results

### Coherence Validation ✅

**Decision compatibility:** All technology choices are mutually compatible. Next.js 15 App Router + Drizzle + Upstash + Anthropic SDK + Zustand + TanStack Query is a well-established 2026 full-stack pattern with no version conflicts. Turbopack is compatible with all chosen libraries. SSE via Next.js Route Handlers runs natively on Vercel serverless — no additional infrastructure needed.

**Pattern consistency:** Naming conventions (snake_case DB, camelCase TS, kebab-case routes) are standard and non-conflicting. SSE event protocol is fully typed. Component boundary rules prevent state ownership conflicts. The `formatILS` and `buildAffiliateUrl` utility mandates ensure cross-cutting concerns are handled uniformly.

**Structure alignment:** Every FR maps to a specific file. The `lib/` domain separation (ai, search, auth, redis, db, utils) mirrors the architectural layers cleanly. No circular dependencies introduced — UI components depend on hooks, hooks depend on lib, lib depends on external services only.

**One deliberate deviation from PRD:** WebSocket → SSE. Documented with rationale. Chat streaming UX is identical to the user; implementation is simpler and more reliable on Vercel.

### Requirements Coverage Validation ✅

**Functional requirements — all 27 covered:**

| Category | FRs | Coverage |
|---|---|---|
| Conversational Interface | FR1–FR7 | ✅ All covered — AI layer + SSE stream + Context Confirmation Card |
| Search & Aggregation | FR8–FR14 | ✅ All covered — Kiwi + Booking clients, aggregator, Redis cache, p-limit |
| Context & Memory | FR15–FR18 | ✅ All covered — Redis session store, constraint extraction, Vercel Cron purge |
| User Accounts | FR19–FR23 | ✅ All covered — anonymous middleware, SaveTripSheet, Twilio, trips API |
| Operations | FR24–FR27 | ✅ All covered — admin dashboard, metrics route, affiliate click tracking |

**Non-functional requirements:**

| NFR | Architecture response |
|---|---|
| 5s end-to-end latency | Parallel `Promise.all` for Kiwi + Booking; Redis cache for repeat queries; Anthropic streaming starts immediately while search runs |
| 60FPS during streaming | Zustand batch updates; `useChat` accumulates chunks in a ref, updates state in RAF-throttled batches |
| TTI < 2.5s on 4G | Next.js code splitting per route; SSR only on landing (not chat); Turbopack optimised bundles |
| 100 concurrent sessions | Vercel serverless scales automatically; each session is stateless (context in Redis); no shared mutable server state |
| 4.5s API timeout | `AbortController` with 4500ms timeout on every Kiwi + Booking fetch; SSE `error` event on timeout with Hebrew message |
| RTL 100% compliance | `dir="rtl"` on `<html>` root; Tailwind `rtl:` variant; rule enforced — never set below `<html>` |
| WCAG 2.1 AA | Verified in UX spec Step 8; Hebrew ARIA labels on all interactive elements; contrast ratios confirmed |
| ±2% ILS accuracy | Exchange rate refreshed daily via Vercel Cron; stored in Redis with 24h TTL; `formatILS` uses cached rate |
| Israeli Privacy Protection Law | 30-day auto-purge via cron; explicit consent checkbox in SaveTripSheet; no PII (passport) storage in schema |
| Affiliate disclosure | `AffiliateDisclosure.tsx` mandatory on every card; `buildAffiliateUrl` enforced by pattern rule |

### Implementation Readiness Validation ✅

**Decision completeness:** All 5 decision categories fully documented with rationale. SSE deviation from PRD explicitly justified. Environment variables listed. Implementation sequence defined (9 steps). No ambiguous "TBD" decisions remaining.

**Structure completeness:** Every FR maps to at least one specific file. All 7 external services have a named client file. Admin, auth, and cron routes are fully specified. No placeholder directories.

**Pattern completeness:** 9 conflict categories addressed. SSE event protocol is typed and wire-format specified. All 8 mandatory rules enumerated. Concrete ✅/❌ examples for every naming convention.

### Gap Analysis

**Critical gaps: none.** All 27 FRs are architecturally covered. No blocking decisions missing.

**Important gaps identified and noted for implementation:**

1. **Exchange Rate API provider not selected.** `EXCHANGE_RATE_API_KEY` is defined but the specific provider (Open Exchange Rates, Fixer.io, or similar) is not chosen. Developer story should pick a free-tier provider with ILS support. Non-blocking — any REST exchange rate API works with the same pattern.

2. **Kiwi and Booking.com API access.** MVP requires approved API keys from both providers. Developer should apply for Kiwi affiliate API access and Booking.com affiliate program in parallel with development. Neither blocks early stories (mock data acceptable until keys arrive).

3. **Hebrew system prompt content.** `lib/ai/systemPrompt.ts` is specified but the prompt text itself is implementation work, not architecture. The prompt must instruct Claude to: extract constraints into the defined `ConfirmationChip` types, respond in Hebrew, emit structured JSON for chips/cards alongside conversational text, and respect the SSE event protocol ordering.

4. **Vercel Cron authentication.** Cron routes (`/api/cron/*`) must validate the `Authorization: Bearer` header that Vercel Cron sends automatically. Implement a `CRON_SECRET` env var check in those routes to prevent unauthorized triggers.

**Nice-to-have (post-MVP):**
- Sentry for error monitoring
- Playwright E2E tests for the core chat flow
- Storybook for component library documentation

### Architecture Completeness Checklist

**✅ Requirements Analysis**
- [x] 27 FRs analyzed for architectural implications
- [x] All NFRs addressed with specific architectural responses
- [x] Technical constraints identified (solo dev, bootstrap budget, Israeli market)
- [x] 8 cross-cutting concerns mapped to patterns and utilities

**✅ Architectural Decisions**
- [x] Database: Neon + Drizzle (with rationale over Supabase/Prisma)
- [x] Caching + sessions: Upstash Redis
- [x] AI streaming: SSE (deviation from PRD WebSocket documented)
- [x] LLM: Claude claude-sonnet-4-6
- [x] Auth: Twilio Verify + JWT httpOnly cookie
- [x] State: Zustand + TanStack Query
- [x] Deployment: Vercel + GitHub CI
- [x] Scheduled jobs: Vercel Cron (2 jobs, free tier)

**✅ Implementation Patterns**
- [x] Naming conventions: DB, API, TypeScript, files
- [x] SSE event protocol fully typed + wire format specified
- [x] State ownership rules per store
- [x] Error language rule (Hebrew user-facing, English logs)
- [x] Price and affiliate URL utility mandates
- [x] 8 mandatory enforcement rules for developer agents

**✅ Project Structure**
- [x] Complete directory tree with every file named
- [x] All 27 FRs mapped to specific files
- [x] All 7 external services mapped to client files
- [x] Component responsibility boundaries defined
- [x] Data flow diagram: user input → SSE results

### Architecture Readiness Assessment

**Overall Status: READY FOR IMPLEMENTATION**

**Confidence level: High**

**Key strengths:**
- Every FR has a named file and layer — no orphaned requirements
- SSE event protocol is typed end-to-end — chat stream cannot diverge between frontend and backend
- Utility mandates (`formatILS`, `buildAffiliateUrl`) enforce the two most legally sensitive cross-cutting concerns
- State ownership rules prevent the most common React complexity trap (distributed chat state)
- All infrastructure choices are free-tier and zero-config — no DevOps work required before first story

**Areas for post-MVP enhancement:**
- Replace Vercel Logs with Sentry for structured error tracking
- Add Playwright E2E covering the full chat → book flow
- Consider Redis Streams if admin metrics volume grows beyond Upstash free tier

### Implementation Handoff

**First command:**
```bash
npx create-next-app@latest tripo --typescript --tailwind --app --src-dir --turbopack
cd tripo
npx shadcn@latest init
```

**Implementation sequence (from Step 4 decisions):**
1. Project init + DB schema + Drizzle migration
2. Redis connection + session middleware
3. Auth flow (Twilio OTP → JWT cookie)
4. `POST /api/chat` SSE route (LLM only, no external APIs)
5. Chat UI (`useChat` hook + streaming rendering)
6. Kiwi + Booking.com integration into `/api/chat`
7. Trip saving + Saved Trips Dashboard
8. Admin dashboard + affiliate click tracking
9. Vercel Cron jobs (exchange rate + purge)

**AI agent directive:** This architecture document is the single source of truth. When making any implementation decision not explicitly covered here, default to: smallest scope, most established pattern, free tier, Hebrew user-facing, English logs.
