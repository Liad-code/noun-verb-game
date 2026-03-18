---
stepsCompleted: [1, 2, 3, 4]
status: complete
completedAt: '2026-03-15'
inputDocuments:
  - _bmad-output/planning-artifacts/prd.md
  - _bmad-output/planning-artifacts/architecture.md
  - _bmad-output/planning-artifacts/ux-design-specification.md
---

# TRIPO - Epic Breakdown

## Overview

This document provides the complete epic and story breakdown for TRIPO, decomposing the requirements from the PRD, UX Design, and Architecture into implementable stories.

## Requirements Inventory

### Functional Requirements

FR1: Users can interact with the system using natural language input in Hebrew.
FR2: The system can parse user constraints (dates, budget in ILS, destinations, amenities, passenger count) from unstructured Hebrew text.
FR3: The system can present curated trip results as interactive UI cards within the chat stream.
FR4: Users can click affiliate action buttons directly from the interactive cards to be redirected to the provider.
FR5: The system can display a "Context Confirmation Card" summarizing understood constraints in Hebrew at the top of results.
FR6: Users can tap individual elements within the "Context Confirmation Card" to edit or correct constraints directly.
FR7: In the event of zero aggregator results, the system outputs a fallback conversational message in Hebrew.
FR8: The system can simultaneously query the Kiwi API for one-way flights originating strictly from Tel Aviv (TLV).
FR9: The system can simultaneously query the Booking.com API for hotel availability matching specified dates and locations.
FR10: The system can filter and aggregate raw API responses into a maximum of 3 cohesive "Trip Packages" per user query.
FR11: The system can dynamically convert and display foreign currency pricing into Israeli New Shekels (ILS).
FR12: The system can fetch and update the ILS exchange rate every 24 hours.
FR13: The system can cache frequent route/destination search results for up to 15 minutes.
FR14: The system can queue incoming API requests to ensure a maximum concurrency limit of 3 simultaneous calls per provider.
FR15: The system can retain all user-stated constraints across multiple turns of dialogue within a single active session.
FR16: The system can automatically apply historical constraints from the current session to new searches without user restating them.
FR17: The system can overwrite a specific constraint while retaining all other previously stated constraints.
FR18: The system automatically purges all conversational memory logs 30 days after session creation.
FR19: Users can initiate and complete an entire trip search session anonymously without an account.
FR20: The system prompts an anonymous user to create an account only when they explicitly choose to "Save" a trip.
FR21: Users can register and authenticate using an Israeli phone number (+972).
FR22: Users can view a "Saved Trips Dashboard" containing all previously saved trip packages and constraints.
FR23: Users can re-initiate a chat session using a saved trip as the baseline context.
FR24: Admin users can access a secure internal dashboard localized in Hebrew.
FR25: Admin users can view latency metrics for Kiwi and Booking.com API integrations.
FR26: Admin users can view the aggregate Click-Through Rate (CTR) for generated affiliate links.
FR27: The system appends affiliate tracking IDs to all outbound Booking.com and Kiwi URLs.

### NonFunctional Requirements

NFR1: The system responds with curated flight and hotel options within 5.0 seconds of the user's conversational prompt.
NFR2: The chat interface renders at 60FPS during AI SSE streaming to prevent UI stuttering.
NFR3: Initial SPA Time to Interactive (TTI) is under 2.5 seconds over a 4G mobile network.
NFR4: The MVP architecture supports up to 100 concurrent chat sessions without exceeding the 5-second latency target.
NFR5: The entire application interface enforces Right-to-Left (RTL) layout without breaking CSS containers.
NFR6: All interactive elements adhere to WCAG 2.1 AA keyboard navigability standards.
NFR7: All dynamic prices, dates, and action buttons feature explicit Hebrew ARIA labels.
NFR8: Text-to-background contrast ratios meet WCAG 2.1 AA minimums (4.5:1 for normal text).
NFR9: If aggregator APIs exceed a 4.5-second timeout, the system gracefully fails and informs the user in Hebrew.
NFR10: Displayed ILS prices maintain ±2% accuracy compared to source API native currency.

### Additional Requirements

**From Architecture:**
- Project bootstrapped with: `npx create-next-app@latest tripo --typescript --tailwind --app --src-dir --turbopack` + `npx shadcn@latest init`
- Neon (PostgreSQL serverless) + Drizzle ORM — schema: users, saved_trips, affiliate_clicks, api_metrics
- Upstash Redis — API result cache (15-min TTL), ILS exchange rate (24h TTL), anonymous session context (30-day TTL)
- Twilio Verify — SMS OTP for Israeli phone numbers (+972)
- Anthropic API (claude-sonnet-4-6) — Hebrew NLP, constraint extraction, response generation
- SSE via Next.js Route Handlers (ReadableStream, text/event-stream) — replaces PRD WebSocket spec
- Zustand (client state) + TanStack Query (server state)
- Vercel Cron (2 jobs): daily ILS rate refresh + nightly 30-day log purge
- Admin route protection via ADMIN_SECRET env var in Next.js middleware
- p-limit library for API concurrency cap (max 3/provider per FR14)
- All affiliate URLs constructed exclusively via buildAffiliateUrl() utility

**From UX Design:**
- dir="rtl" set once on <html> root in layout.tsx — never on individual components
- Warm Cold-Start: animated Hebrew greeting + 3 tappable suggestion chips on empty session
- 100dvh layout with env(safe-area-inset-bottom) for iOS Safari keyboard avoidance
- Typing indicator (3-dot Framer Motion bounce) before streaming begins
- Sequential streaming status messages ("מחפש טיסות...") via Framer Motion fade-in
- Context Confirmation Card: 9 chip types, bottom sheet editors on mobile, inline toggles for simple constraints
- Constraint Memory Pulse: #A5F3FC cyan flash → #EEF3FD over 300ms on constraint update
- Trip Package Card: 6 zones — header+badge, price hero, flights, hotel+amenities, dual CTAs, footer
- Framer Motion card entrance: staggered fade-up (0ms / 150ms / 300ms)
- Sign-up bottom sheet: 2-phase (phone → OTP), identical to WhatsApp flow
- Affiliate disclosure text mandatory on every Trip Package Card (Israeli Consumer Protection Law)
- Hebrew ARIA labels on all interactive elements throughout

### FR Coverage Map

| Requirement | Epic |
|---|---|
| FR1 (Hebrew NLP input) | Epic 2 |
| FR2 (Constraint parsing) | Epic 2 |
| FR3 (Trip result cards in chat) | Epic 2 |
| FR4 (Affiliate action buttons) | Epic 2 |
| FR5 (Context Confirmation Card display) | Epic 2 |
| FR6 (Editable constraint chips) | Epic 2 |
| FR7 (Zero-results fallback message) | Epic 2 |
| FR8 (Kiwi API — TLV flights) | Epic 2 |
| FR9 (Booking.com API — hotels) | Epic 2 |
| FR10 (Aggregate max 3 Trip Packages) | Epic 2 |
| FR11 (ILS currency conversion display) | Epic 2 |
| FR12 (ILS exchange rate fetch) | Epic 2 + Epic 4 |
| FR13 (15-min result cache) | Epic 2 |
| FR14 (API concurrency cap) | Epic 2 |
| FR15 (Constraint retention across turns) | Epic 2 |
| FR16 (Auto-apply historical constraints) | Epic 2 |
| FR17 (Overwrite single constraint) | Epic 2 |
| FR18 (30-day log auto-purge) | Epic 4 |
| FR19 (Anonymous session) | Epic 1 + Epic 2 + Epic 3 |
| FR20 (Save-trigger account prompt) | Epic 3 |
| FR21 (Israeli phone number auth) | Epic 3 |
| FR22 (Saved Trips Dashboard) | Epic 3 |
| FR23 (Re-initiate from saved trip) | Epic 3 |
| FR24 (Hebrew Admin Dashboard) | Epic 4 |
| FR25 (API latency metrics) | Epic 4 |
| FR26 (Affiliate CTR metrics) | Epic 4 |
| FR27 (Affiliate tracking IDs) | Epic 2 + Epic 4 |
| NFR1 (5s response latency) | Epic 2 |
| NFR2 (60FPS SSE rendering) | Epic 2 |
| NFR3 (TTI < 2.5s) | Epic 1 |
| NFR4 (100 concurrent sessions) | Epic 2 |
| NFR5 (RTL layout enforcement) | Epic 1 |
| NFR6 (WCAG 2.1 AA keyboard nav) | Epic 2 |
| NFR7 (Hebrew ARIA labels) | Epic 2 |
| NFR8 (Contrast ratios WCAG AA) | Epic 1 |
| NFR9 (4.5s API timeout + Hebrew fallback) | Epic 2 |
| NFR10 (ILS ±2% accuracy) | Epic 2 |

## Epic List

| # | Title | Goal | FRs Covered |
|---|---|---|---|
| 1 | Project Foundation | Bootstrap the Next.js project with full RTL Hebrew configuration, design tokens, database schema, Redis connection, environment scaffold, and anonymous session middleware — establishing the complete technical bedrock every other epic builds on. | FR19 (anonymous session foundation), NFR3, NFR5, NFR8 |
| 2 | Conversational Trip Discovery | Deliver the entire AI-powered chat experience: Hebrew NLP via Claude, constraint extraction and memory, simultaneous Kiwi + Booking.com API calls, ILS currency conversion, Trip Package Card rendering, Context Confirmation Card, SSE streaming with status indicators, affiliate URL construction, and all error/fallback handling. | FR1–FR17, FR19, FR27 (partial), NFR1–NFR2, NFR4, NFR6–NFR7, NFR9–NFR10 |
| 3 | Account Creation & Trip Saving | Enable authenticated user accounts via Israeli SMS OTP (Twilio Verify), the save-trip flow triggered from chat cards, the Saved Trips Dashboard, and the ability to re-initiate a chat from a saved trip. | FR19–FR23 |
| 4 | Platform Operations & Compliance | Implement the Hebrew Admin Dashboard with API latency and CTR metrics, Vercel Cron jobs for daily ILS rate refresh and nightly 30-day log purge, affiliate click-tracking endpoint, and all compliance/regulatory requirements. | FR12 (cron automation), FR18, FR24–FR27 (completion), NFR3 (Cron reliability) |

---

## Epic 1: Project Foundation

**Goal:** Bootstrap the Next.js project with full RTL Hebrew configuration, design tokens, database schema, Redis connection, environment scaffold, and anonymous session middleware.

**FRs Covered:** FR19 (anonymous session foundation), NFR3, NFR5, NFR8

---

### Story 1.1: Next.js Project Bootstrap & RTL Hebrew Foundation

As a **developer**,
I want the Next.js 15 project scaffolded with TypeScript, Tailwind CSS, shadcn/ui, Turbopack, and `dir="rtl"` set on the `<html>` root with the Rubik font,
So that every subsequent story builds on a consistent, RTL-compliant, standards-correct foundation.

**Acceptance Criteria:**

**Given** the bootstrap commands are run (`npx create-next-app@latest tripo --typescript --tailwind --app --src-dir --turbopack` + `npx shadcn@latest init`)
**When** I run `npm run dev`
**Then** the app loads at `localhost:3000`, the `<html>` element has `dir="rtl" lang="he"`, and Rubik font is loaded from Google Fonts
**And** the page background renders `#FAFAF8` (warm off-white) and Hebrew text flows right-to-left without broken containers
**And** `dir="rtl"` is set exclusively on the `<html>` root in `src/app/layout.tsx` — never on individual components (NFR5)

---

### Story 1.2: Design Token System & Global Styles

As a **developer**,
I want `tailwind.config.ts` extended with all TRIPO design tokens (Authority Blue palette, semantic aliases, chip-pulse keyframe) and shadcn/ui `:root` CSS variables configured,
So that every UI component can use consistent, spec-compliant color values as Tailwind utility classes.

**Acceptance Criteria:**

**Given** `tailwind.config.ts` is updated with the TRIPO color extension
**When** I inspect the compiled CSS
**Then** custom color utilities are available: `brand-blue` (#1B4FD8), `brand-cyan` (#A5F3FC), and the `chip-pulse` keyframe animation (300ms #A5F3FC → #EEF3FD) is defined in `globals.css`
**And** shadcn/ui `:root` CSS variables in `globals.css` map correctly (e.g. `--primary: 220 82% 47%`)
**And** WCAG AA contrast ratio of 4.5:1 minimum is verified for all text-on-background token combinations (NFR8)

---

### Story 1.3: Database Schema & Drizzle ORM Setup

As a **developer**,
I want the Neon PostgreSQL database connected via Drizzle ORM with all four core tables defined (`users`, `saved_trips`, `affiliate_clicks`, `api_metrics`),
So that authenticated features, trip saving, analytics, and admin metrics have a ready data layer.

**Acceptance Criteria:**

**Given** `DATABASE_URL` (Neon connection string) is set in `.env.local`
**When** I run `npx drizzle-kit push`
**Then** all four tables are created in Neon with correct columns, data types, and indexes:
- `users`: `id`, `phone_number`, `created_at`, `consent_ai_memory`
- `saved_trips`: `id`, `user_id`, `trip_data` (jsonb), `constraints` (jsonb), `created_at`
- `affiliate_clicks`: `id`, `session_id`, `user_id` (nullable), `provider`, `url`, `clicked_at`
- `api_metrics`: `id`, `provider`, `latency_ms`, `status_code`, `requested_at`

**And** `src/lib/db/schema.ts` exports typed Drizzle table definitions using snake_case column names
**And** `src/lib/db/index.ts` exports a singleton database client

---

### Story 1.4: Upstash Redis Configuration & Cache Utilities

As a **developer**,
I want Upstash Redis connected with typed helper utilities for the three cache namespaces,
So that all caching operations across the app use a consistent, type-safe interface with correct TTLs.

**Acceptance Criteria:**

**Given** `UPSTASH_REDIS_REST_URL` and `UPSTASH_REDIS_REST_TOKEN` are set in `.env.local`
**When** the `src/lib/redis.ts` cache utility functions are called
**Then** `setApiCache(key, data)` stores with 15-minute TTL, `setExchangeRate(rate)` stores with 24-hour TTL, and `setSessionContext(sessionId, context)` stores with 30-day TTL
**And** each corresponding getter returns `null` on cache miss and correctly typed data on cache hit
**And** the Upstash Redis client is exported as a singleton from `src/lib/redis.ts`

---

### Story 1.5: Anonymous Session Middleware & Environment Scaffold

As an **anonymous visitor**,
I want a session automatically created for me on my first visit (no login required) and admin routes protected from unauthorized access,
So that my constraints are persisted throughout my planning session and platform operations remain secure.

**Acceptance Criteria:**

**Given** a new visitor arrives with no session cookie
**When** `src/middleware.ts` processes the request
**Then** a UUID session ID is generated, set as an `httpOnly`, `SameSite=Lax` cookie with 30-day expiry, and the session is initialized in Redis
**And** on subsequent requests the existing session cookie is read and forwarded as `x-session-id` header to Route Handlers
**And** any request to `/admin/*` without a matching `ADMIN_SECRET` value returns HTTP 401
**And** `.env.local.example` documents all required environment variables: `DATABASE_URL`, `UPSTASH_REDIS_REST_URL`, `UPSTASH_REDIS_REST_TOKEN`, `ANTHROPIC_API_KEY`, `TWILIO_ACCOUNT_SID`, `TWILIO_AUTH_TOKEN`, `TWILIO_VERIFY_SID`, `ADMIN_SECRET`, `CRON_SECRET`, `NEXT_PUBLIC_APP_URL`

---

## Epic 2: Conversational Trip Discovery

**Goal:** Deliver the complete AI-powered chat experience end-to-end — Hebrew NLP, constraint memory, live API searches, ILS pricing, and rendered Trip Package Cards.

**FRs Covered:** FR1–FR17, FR19, FR27 (partial) | **NFRs:** NFR1–NFR2, NFR4, NFR6–NFR7, NFR9–NFR10

---

### Story 2.1: Chat Interface Layout & Warm Cold-Start

As a **user**,
I want a full-screen Hebrew chat interface that greets me with an animated welcome message and 3 tappable suggestion chips when I first open the app,
So that I feel immediately oriented and can start planning without needing to know what to type.

**Acceptance Criteria:**

**Given** a user opens the app with a new empty session
**When** the `/` route renders
**Then** the layout uses `100dvh` height with `env(safe-area-inset-bottom)` padding for iOS Safari keyboard avoidance, and the header shows the TRIPO logo
**And** a Warm Cold-Start sequence plays: animated Hebrew greeting fades in, then 3 tappable suggestion chips appear (e.g. "טיסה לברצלונה 🏖", "חופשה משפחתית באפריל", "סוף שבוע רומנטי בפריז")
**And** tapping a suggestion chip populates the input bar with that text
**And** the input bar font-size is `16px` minimum (prevents iOS Safari auto-zoom on focus)
**And** all interactive elements have Hebrew ARIA labels (NFR7) and are keyboard-navigable (NFR6)

---

### Story 2.2: SSE Chat Route Handler & Claude Hebrew Streaming

As a **user**,
I want my Hebrew messages to receive a streamed AI response with live status updates before the full answer arrives,
So that I see the system is working and get a responsive, conversational experience.

**Acceptance Criteria:**

**Given** the user submits a Hebrew message
**When** `POST /api/chat` is called
**Then** the route responds with `Content-Type: text/event-stream` and a `ReadableStream` emitting typed SSE events: `status`, `text`, `chips`, `card`, `done`, `error`
**And** a 3-dot Framer Motion bounce typing indicator appears before the first SSE event arrives
**And** sequential Hebrew status messages ("מחפש טיסות...", "בודק מלונות...", "מסדר הכל בשבילך...") are emitted as `status` events and fade in via Framer Motion
**And** Claude `claude-sonnet-4-6` responds in Hebrew using a system prompt establishing it as an Israeli travel assistant
**And** the chat UI renders at 60FPS during streaming with no dropped frames (NFR2)
**And** if no response arrives within 4.5 seconds the stream emits an `error` event with a Hebrew fallback message (NFR9)

---

### Story 2.3: Constraint Extraction & Session Memory

As a **user**,
I want the system to remember everything I tell it across my entire chat session without me repeating myself,
So that I can have a natural conversation and correct just one thing at a time.

**Acceptance Criteria:**

**Given** the user states constraints in Hebrew (e.g. "תקציב 6,000 שקל, שני מבוגרים, לפוקט בסוף יולי")
**When** Claude processes the message
**Then** structured constraints are extracted (budget, passengers, destination, dates) and stored as JSON in the Redis session under `x-session-id` (FR15)
**And** on the next turn, previously stored constraints are injected into the Claude context automatically without the user restating them (FR16)
**And** if the user overrides one constraint (e.g. "שנה את התקציב ל-8,000"), only that field is updated — all others are retained (FR17)
**And** the Constraint Memory Pulse (#A5F3FC cyan flash → #EEF3FD over 300ms) triggers on any constraint update in the UI

---

### Story 2.4: Context Confirmation Card Component

As a **user**,
I want to see a card summarizing exactly what the system understood from my requests, with the ability to tap any chip to correct it instantly,
So that I never have to wonder whether the AI understood me correctly.

**Acceptance Criteria:**

**Given** constraints have been extracted and trip results are being displayed
**When** the Context Confirmation Card renders (via `chips` SSE event)
**Then** it displays chips for all 9 constraint types present: destination, departure-date, return-date, budget, passengers, accommodation-type, amenities, flight-type, flexibility
**And** each chip has 4 visual states: default (filled), editable (outlined + pencil icon), updating (cyan pulse), unmet (muted + warning icon)
**And** tapping an editable chip on mobile opens a bottom sheet editor; on desktop it opens an inline popover
**And** submitting a chip edit triggers a constraint update in Redis and re-emits a `chips` SSE event with the Constraint Memory Pulse animation
**And** all chip interactive elements have Hebrew ARIA labels (NFR7) and are keyboard-navigable (NFR6)

---

### Story 2.5: Kiwi Flight API Integration

As a **user**,
I want the system to search for real one-way flights from Tel Aviv (TLV) to my destination,
So that I get accurate, live flight options with prices.

**Acceptance Criteria:**

**Given** the user's constraints include a destination and travel dates
**When** the chat route handler triggers a flight search
**Then** `src/lib/api/kiwi.ts` calls the Kiwi API for one-way flights departing strictly from TLV (FR8)
**And** a maximum of 3 concurrent Kiwi calls are enforced via `p-limit(3)` (FR14)
**And** results are cached in Redis for 15 minutes with key `kiwi:{route}:{date}` (FR13)
**And** if Kiwi does not respond within 4.5 seconds, the request is aborted and an `error` SSE event emits a Hebrew message: "לא הצלחנו לטעון טיסות כרגע, נסה שוב" (NFR9)
**And** raw Kiwi response data is typed via a `KiwiFlightResult` TypeScript interface

---

### Story 2.6: Booking.com Hotel API Integration

As a **user**,
I want the system to search for real hotel availability at my destination for my dates,
So that I get accurate, live hotel options with prices.

**Acceptance Criteria:**

**Given** the user's constraints include a destination and stay dates
**When** the chat route handler triggers a hotel search
**Then** `src/lib/search/booking.ts` calls the Booking.com API for hotels matching the specified location and dates (FR9)
**And** a maximum of 3 concurrent Booking.com calls are enforced via `p-limit(3)` via `bookingLimiter` from `src/lib/utils/rateLimit.ts` (FR14)
**And** results are cached in Redis for 15 minutes with key `booking:{destination}:{checkin}:{checkout}` (FR13)
**And** if Booking.com does not respond within 4.5 seconds, the request is aborted and a `BookingTimeoutError` is thrown (NFR9)
**And** raw Booking.com response data is typed via a `BookingHotelResult` TypeScript interface exported from `booking.ts`
**And** every outbound Booking.com URL passes through `buildAffiliateUrl(url, 'booking')` (FR27)
**And** `searchBookingHotels()` runs concurrently with `searchKiwiFlights()` in the route handler via `Promise.all` (Story 2.5 pattern)
**And** co-located tests at `src/lib/search/booking.test.ts` cover: missing API key returns [], cache hit skips fetch, timeout throws BookingTimeoutError, successful fetch maps to HotelResult[], cache is written after fetch

---

### Story 2.7: ILS Currency Conversion Utility

As a **user**,
I want all prices shown to me in Israeli Shekels (₪) alongside the original currency,
So that I can immediately understand costs without mental conversion.

**Acceptance Criteria:**

**Given** flight or hotel prices are returned in a foreign currency (e.g. EUR, USD)
**When** `formatILS(amount, sourceCurrency)` is called from `src/lib/utils/formatILS.ts`
**Then** the function reads the current exchange rate from Redis (`getExchangeRate()`) and returns a formatted string: e.g. "₪2,340 (~€580)"
**And** if no exchange rate is cached, the function uses the hardcoded fallback rates already present in `kiwi.ts` (to be extracted into a shared constant)
**And** the converted ILS price stays within ±2% accuracy of the source currency value (NFR10)
**And** `formatILS()` is the only function used throughout the codebase to display prices in the Trip tab — never raw `Intl.NumberFormat` calls in components
**And** `PriceDisplay` in `src/components/shared/PriceDisplay.tsx` wraps `formatILS()` for all Trip tab card rendering

---

### Story 2.8: Trip Package Aggregation & Trip Tab Rendering

As a **user**,
I want to see up to 3 complete "Trip Packages" — each combining a flight and hotel — presented as rich cards in the "הטיול שלי" tab, with buttons to book each directly,
So that I can make an informed decision and book in a single tap without leaving the app.

**Acceptance Criteria:**

**Given** Kiwi flight results and Booking.com hotel results are available
**When** `src/lib/search/aggregator.ts` processes them
**Then** a maximum of 3 cohesive `TripPackage` objects are produced, each pairing one flight with one hotel (FR10)
**And** the route handler emits `{ type: 'results', payload: { packages: TripPackage[] } }` as a single SSE batch event
**And** the route handler also emits a Hebrew AI notification message via `text`/`done` events: "מצאתי אפשרויות! עבור ללשונית הטיול לראות את התוצאות ✈️"
**And** `useChat` handles the `results` event by calling `setTripPackages(packages)` on chatStore and `setTripTabHasNewResults(true)` on uiStore
**And** `TripTab` (`src/components/trip/TripTab.tsx`) renders `TripPackageCard` × 1–3 when packages are present
**And** each Trip Package Card renders 6 zones: header+destination badge, ILS price hero, flight details, hotel details+amenity chips, dual CTA buttons ("הזמן טיסה" / "הזמן מלון"), footer with affiliate disclosure
**And** CTA buttons use `buildAffiliateUrl(url, provider)` exclusively to construct all outbound URLs (FR4, FR27)
**And** the affiliate disclosure reads: "טריפו הינה פלטפורמת השוואה. ההזמנה מתבצעת ישירות מול הספק" on every card
**And** cards animate in with staggered Framer Motion fade-up: card 1 at 0ms, card 2 at 150ms, card 3 at 300ms
**And** if zero results are returned, the `results` event is not emitted; instead a conversational Hebrew fallback `text` SSE event is sent (FR7)
**And** all card interactive elements have Hebrew ARIA labels and meet WCAG AA keyboard navigation (NFR6, NFR7)
**And** the end-to-end latency from user message to cards visible in Trip tab is under 5 seconds (NFR1)
**And** ❌ No TripPackageCard, price, or CTA button ever renders inside the Chat tab

---

## Epic 3: Account Creation & Trip Saving

**Goal:** Enable authenticated user accounts via Israeli SMS OTP, save trips from chat cards, Saved Trips Dashboard, and re-initiate chat from a saved trip.

**FRs Covered:** FR19–FR23

---

### Story 3.1: Save Trip Trigger & Sign-Up Bottom Sheet (Anonymous Path)

As an **anonymous user**,
I want to be prompted to create an account only when I tap "שמור טיול" on a Trip Package Card — not before,
So that I can explore freely and only sign up when I decide to save something I love.

**Acceptance Criteria:**

**Given** an anonymous user (no auth cookie) taps "שמור טיול" on a Trip Package Card
**When** the save action fires
**Then** a sign-up bottom sheet slides up (2-phase: phone entry → OTP) without navigating away from the chat (FR19, FR20)
**And** no account prompt appears at any other point in the session — only on explicit save intent
**And** if the user dismisses the bottom sheet, the chat state is fully preserved and no data is lost
**And** for already-authenticated users tapping "שמור טיול", the bottom sheet is skipped and the trip saves immediately

---

### Story 3.2: Phone Number Entry & Twilio OTP Dispatch

As a **new user**,
I want to enter my Israeli phone number and receive an SMS verification code,
So that I can create an account using the same familiar flow as WhatsApp.

**Acceptance Criteria:**

**Given** the sign-up bottom sheet Phase 1 is open
**When** the user enters a valid +972 Israeli phone number and taps "שלח קוד"
**Then** `POST /api/auth/send-otp` calls Twilio Verify to dispatch an SMS OTP to the number (FR21)
**And** the bottom sheet transitions to Phase 2 (OTP entry) with a 6-digit input field
**And** if the phone number format is invalid (not +972 prefix), a Hebrew inline error is shown: "מספר טלפון לא תקין"
**And** if Twilio returns an error, a Hebrew message is shown: "שגיאה בשליחת הקוד, נסה שוב"
**And** the phone input uses `type="tel"` and `inputmode="numeric"` for the correct mobile keyboard

---

### Story 3.3: OTP Verification & Account Creation

As a **new user**,
I want to enter the SMS code I received to verify my phone and create my account in one step,
So that I'm registered and logged in immediately — no passwords, no forms.

**Acceptance Criteria:**

**Given** the user is on Phase 2 (OTP entry) of the sign-up bottom sheet
**When** the user enters the 6-digit OTP and taps "אמת"
**Then** `POST /api/auth/verify-otp` calls Twilio Verify to validate the code
**And** on success: a new `users` row is created in the DB (if first time), a JWT is issued and set as an `httpOnly` `SameSite=Lax` cookie, and the anonymous session is migrated by linking its `session_id` to the new `user_id`
**And** an explicit consent checkbox for AI memory storage is shown and must be checked before account creation completes (Israeli Privacy Protection Law)
**And** if the OTP is incorrect, a Hebrew error is shown: "הקוד שגוי, נסה שוב"
**And** if the OTP has expired, a Hebrew error is shown with a "שלח שוב" resend link

---

### Story 3.4: Trip Saving to Database

As an **authenticated user**,
I want my chosen Trip Package to be saved to my account with all my session constraints,
So that I can find it later without starting from scratch.

**Acceptance Criteria:**

**Given** an authenticated user taps "שמור טיול" (or just completed OTP verification)
**When** `POST /api/trips/save` is called
**Then** a new row is inserted into `saved_trips` with the full `trip_data` (jsonb) and `constraints` (jsonb) linked to the authenticated `user_id`
**And** the Trip Package Card updates to show a "נשמר ✓" confirmation state inline
**And** if the save fails, a Hebrew toast is shown: "שמירה נכשלה, נסה שוב"
**And** saving the same trip twice is handled gracefully (idempotent upsert — no duplicate rows)

---

### Story 3.5: Saved Trips Dashboard

As an **authenticated user**,
I want a dashboard showing all my previously saved trips as cards,
So that I can review my options and pick up where I left off.

**Acceptance Criteria:**

**Given** an authenticated user navigates to `/dashboard`
**When** the page loads
**Then** `GET /api/trips` returns all saved trips for the current user ordered by `created_at` descending
**And** each saved trip renders as a summary card showing: destination, travel dates, total ILS price, and a "המשך תכנון" button
**And** if the user has no saved trips, a Hebrew empty state is shown: "עדיין לא שמרת טיולים — התחל לתכנן!"
**And** unauthenticated users accessing `/dashboard` are redirected to `/` by the Next.js middleware

---

### Story 3.6: Re-Initiate Chat from Saved Trip

As an **authenticated user**,
I want to tap "המשך תכנון" on a saved trip and have a new chat session open with all my original constraints pre-loaded,
So that I can refine or re-search without re-explaining everything.

**Acceptance Criteria:**

**Given** the user taps "המשך תכנון" on a saved trip card in the dashboard
**When** `POST /api/chat/resume` is called with the saved trip's `constraints` payload
**Then** a new Redis session is created with the saved constraints pre-populated as session context (FR23)
**And** the user is navigated to `/` where the Context Confirmation Card renders immediately showing the restored constraints with the Constraint Memory Pulse animation
**And** the chat input is focused and a Hebrew message appears: "ממשיכים מאיפה שעצרנו — מה תרצה לשנות?"
**And** all subsequent searches in the new session automatically apply the restored constraints (FR16)

---

## Epic 4: Platform Operations & Compliance

**Goal:** Affiliate click tracking, Vercel Cron automation (daily ILS rate refresh + nightly log purge), Hebrew Admin Dashboard with API latency and CTR metrics.

**FRs Covered:** FR12 (cron automation), FR18, FR24–FR27

---

### Story 4.1: Affiliate Click Tracking Endpoint

As a **business owner**,
I want every affiliate link click recorded to the database,
So that I have accurate data on which trips and providers generate revenue.

**Acceptance Criteria:**

**Given** a user taps a CTA button ("הזמן טיסה" / "הזמן מלון") on a Trip Package Card
**When** the click fires before the browser redirects to the provider
**Then** `POST /api/affiliate/click` inserts a row into `affiliate_clicks` with: `session_id`, `user_id` (nullable), `provider` ("kiwi" | "booking"), `url`, `clicked_at` (FR27)
**And** the API responds within 200ms so the redirect is not perceptibly delayed
**And** `buildAffiliateUrl(provider, params)` remains the sole constructor for all outbound URLs — affiliate tracking IDs are always appended
**And** if the tracking insert fails, the redirect still proceeds — click tracking must never block the user

---

### Story 4.2: Daily ILS Exchange Rate Cron Job

As the **platform**,
I want the ILS exchange rate refreshed automatically every 24 hours,
So that displayed prices remain accurate without manual intervention.

**Acceptance Criteria:**

**Given** a Vercel Cron job is configured in `vercel.json` to run daily (e.g. `0 6 * * *`)
**When** `GET /api/cron/refresh-exchange-rate` is called by the Cron scheduler
**Then** the endpoint validates the `Authorization: Bearer {CRON_SECRET}` header and returns 401 if invalid
**And** on valid request: fetches the current ILS exchange rate from the configured exchange rate API and writes it to Redis via `setExchangeRate(rate)` with 24-hr TTL (FR12)
**And** the result is logged to `api_metrics` (provider: "exchange-rate", latency_ms, status_code)
**And** if the external rate API fails, the existing cached rate is retained and the failure is logged — the app does not crash

---

### Story 4.3: Nightly 30-Day Session Log Purge Cron Job

As the **platform**,
I want all conversational session data older than 30 days automatically deleted every night,
So that TRIPO complies with Israeli Privacy Protection Law without manual maintenance.

**Acceptance Criteria:**

**Given** a Vercel Cron job is configured in `vercel.json` to run nightly (e.g. `0 2 * * *`)
**When** `GET /api/cron/purge-sessions` is called by the Cron scheduler
**Then** the endpoint validates the `Authorization: Bearer {CRON_SECRET}` header and returns 401 if invalid
**And** on valid request: all Redis anonymous session keys older than 30 days are deleted (FR18)
**And** corresponding rows in `saved_trips` for deleted anonymous sessions are also purged
**And** the count of purged sessions is logged for audit purposes
**And** authenticated users' saved trips are NOT purged — only anonymous session context data

---

### Story 4.4: Hebrew Admin Dashboard — API Health Metrics

As an **admin user**,
I want a secure Hebrew dashboard showing real-time API latency metrics for Kiwi and Booking.com,
So that I can identify integration issues before they impact users.

**Acceptance Criteria:**

**Given** the admin navigates to `/admin` with a valid `ADMIN_SECRET` value
**When** the admin dashboard loads
**Then** the page is fully in Hebrew with RTL layout (FR24)
**And** it displays a latency metrics panel for each provider sourced from `api_metrics`: average latency (last 24h), p95 latency, error rate, and last checked timestamp (FR25)
**And** metrics refresh on page load (no live polling required for MVP)
**And** if a provider's average latency exceeds 4.0 seconds, the row is highlighted as a warning
**And** unauthorized access to `/admin/*` returns HTTP 401 (enforced by middleware from Story 1.5)

---

### Story 4.5: Hebrew Admin Dashboard — Affiliate CTR Metrics

As an **admin user**,
I want the admin dashboard to show aggregate Click-Through Rate for all generated affiliate links,
So that I can track monetization performance against business success criteria.

**Acceptance Criteria:**

**Given** the admin is on the `/admin` dashboard
**When** the CTR metrics section loads
**Then** it displays: total affiliate clicks (all time), clicks by provider (Kiwi vs. Booking.com), clicks in the last 7 and 30 days, and CTR percentage (clicks ÷ sessions with cards shown) (FR26)
**And** all figures are sourced from the `affiliate_clicks` table via `GET /api/admin/metrics`
**And** the metrics endpoint validates `ADMIN_SECRET` and returns 401 for unauthenticated requests
**And** all labels, headings, and data descriptions are in Hebrew

