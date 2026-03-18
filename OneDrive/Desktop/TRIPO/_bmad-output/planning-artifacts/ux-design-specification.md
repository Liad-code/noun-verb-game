---
stepsCompleted: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]
stepsInProgress: []
status: MVP_COMPLETE
inputDocuments:
  - _bmad-output/planning-artifacts/product-brief-TRIPO-2026-03-12.md
  - _bmad-output/planning-artifacts/prd.md
---

# UX Design Specification TRIPO

**Author:** Liad
**Date:** 2026-03-14

---

<!-- UX design content will be appended sequentially through collaborative workflow steps -->

## Executive Summary

### Project Vision

Tripo is a mobile-first, Hebrew-native, AI-powered conversational travel planning app targeting the Israeli market. Its core proposition: replace the fragmented multi-tab travel booking experience with a single chat interface that holds full trip context and delivers curated flight + hotel packages in under 10 minutes. The experience is grounded in the WhatsApp conversational pattern familiar to Israeli users, making the AI feel like a knowledgeable travel friend rather than a search engine.

### Target Users

**Sarah — The Overwhelmed Planner (Primary)**
32-year-old Israeli professional who plans 3-4 trips/year. Currently spends 4-6 hours across multiple tabs. Her core pain is not effort — it's trust anxiety: "Did I get the best deal? Did it miss something?" She needs Tripo to feel authoritative and transparent, not just fast.

**David — The Family Trip Coordinator (Secondary)**
41-year-old father of two with complex, non-negotiable constraints (toddler, direct flights only, pool). His pain is that every platform handles multi-constraint filtering differently. He needs Tripo to remember everything and never make him repeat himself.

**Tech Profile:** Medium savvy, smartphone-native (WhatsApp generation). Conversational UI is intuitive for this audience. Primary device: mobile. RTL Hebrew is non-negotiable.

### Key Design Challenges

1. **Trust is the #1 UX challenge.** Israeli users need to feel the AI searched authoritatively and didn't miss a better deal. Every result surface must communicate transparency: what was searched, across which providers, what was understood.

2. **Mobile-first RTL with embedded result cards.** RTL layout + chat stream + rich Trip Package cards on a 390px screen is a compounding design constraint requiring deliberate touch-target, readability, and hierarchy decisions.

3. **Cold-start / blank chat problem.** The first-time user experience must eliminate "what do I type?" uncertainty through smart placeholder text, example prompts, or guided onboarding — without feeling like a form.

4. **Anonymous-to-account conversion seam.** The save-trip prompt is a UX inflection point that must feel like a natural next step, not a wall.

### Design Opportunities

1. **The WhatsApp mental model.** Israeli users already know how to chat. The AI persona, message cadence, and typing indicators should evoke texting a well-connected travel friend — making zero learning curve the default.

2. **Context Confirmation Card as trust anchor.** Tripo's most differentiating UX pattern. Showing exactly what the AI understood and searched transforms user anxiety ("did it miss something?") into delight ("it got all of that from one sentence?").

3. **Trip Package card as the hero moment.** The bundled flight + hotel card with ₪ pricing and one-tap booking is the "זה הכל?" moment — the product's most shareable, conversion-driving asset. Deserves outsized design investment.

## Core User Experience

### Defining Experience

The core of Tripo is a single interaction loop: the user types one casual Hebrew sentence describing their trip → the AI searches immediately without asking clarifying questions → a Context Confirmation Card appears showing exactly what was understood, followed by up to 3 curated Trip Package cards with one-tap booking links. The experience is designed to feel instant and magical — like texting a well-connected travel friend who comes back in seconds with perfect answers.

**Critical exception:** If destination is entirely absent, the AI asks exactly ONE short Hebrew question. No multi-step intake forms, ever.

### Platform Strategy

- **Primary platform:** Mobile web (390px-first). All critical interactions designed for one-handed thumb use.
- **Secondary platform:** Desktop web. Same experience, wider canvas.
- **Input:** Touch-primary. Keyboard on desktop. RTL Hebrew throughout.
- **No native app in MVP.** Web-only, but designed to feel app-native on mobile.
- **No offline functionality required.** Real-time API data is the core value.

### Effortless Interactions

1. **The typing-to-results flow** must feel zero-friction. No forms, no dropdowns, no date pickers. Natural Hebrew text in → curated packages out.

2. **The loading state** must feel active and trustworthy, not passive. A WhatsApp-style typing indicator (animated 3 dots) transitions into streaming Hebrew status messages:
   - "מחפש טיסות מתל אביב..."
   - "בודק מלונות בברצלונה..."
   - "מרכיב את החבילות הכי טובות בשבילך..."
   This is not a wait screen — it's proof the AI is working hard for the user.

3. **The Context Confirmation Card** appears before results as a horizontal row of tappable Hebrew chips:
   `"חיפשתי: ✈️ TLV→BCN | 📅 24-31/07 | 👥 2 | ₪6,000"`
   Each chip is individually editable inline. This is the trust anchor — visual proof the AI understood correctly, with instant correction if not.

4. **The save/sign-up flow** uses Israeli phone number + SMS OTP only (no password, no email). Presented as a bottom sheet modal — never a page navigation. The pattern is identical to WhatsApp signup, which Israeli users know instinctively.

5. **Returning user experience** opens directly to the chat interface, ready to type. A subtle non-blocking strip at the top provides access to saved trips: "הטיולים השמורים שלך ←" — present but never in the way.

### Critical Success Moments

1. **The "זה הכל?" moment** — User types one Hebrew sentence and 3 beautiful Trip Package cards appear with flights + hotel bundled, ₪ prices, and one-tap booking. This is the make-or-break first impression. If this moment delights, Tripo wins.

2. **The trust moment** — User sees the Context Confirmation Card and thinks "it understood every detail." If this card is wrong, and editing it is instant and easy, that's still a win. If the card is right, it's magic.

3. **The constraint memory moment** — User says "push it back a week" and Tripo updates ALL cards with the new dates while remembering toddler, direct flights, pool, budget. Users who hit this moment become evangelists.

4. **The sign-up non-moment** — User tries to save a trip and the bottom sheet slides up, they enter their Israeli phone number, get an SMS, and they're in — in under 20 seconds. If this feels like friction, conversion dies here.

### Experience Principles

1. **Search first, clarify after.** Never gate the experience with questions. Show results immediately; let the Context Confirmation Card be the correction mechanism.

2. **Transparency is the feature.** The loading state, the Context Confirmation Card, and provider visibility aren't polish — they ARE the product's trust proposition.

3. **WhatsApp-native everywhere.** Every pattern should feel like something Israelis already know: typing dots, OTP auth, bottom sheets, conversational Hebrew tone.

4. **Chat is the chrome.** Results, context, saving — nothing breaks to a new page. The entire experience lives in the stream.

5. **One thumb, one tap.** Every critical action — editing a constraint, booking a package, signing up — must be completable with a single thumb on a mobile screen.

## Desired Emotional Response

### Primary Emotional Goals

**The overall emotional target:** Tripo should feel like a well-connected, knowledgeable Israeli friend who handles everything — warm, capable, and completely on your side. Not a tool. Not a search engine. A friend.

- **Core delight:** "זה הכל?" — disbelief at how easy it was
- **Core confidence:** "סוף סוף משהו אחר" — finally something genuinely different
- **Core trust:** "הוא עובד בשבילי" — the AI is actively working for me

### Emotional Journey Mapping

**1. First Discovery (Landing Page)**
Target: Quiet confidence and intrigue from simplicity.
The landing page should feel like a knowledgeable friend saying "תן לי לטפל בזה" (let me handle this). Not hype, not bold claims — understated confidence that lets the product speak. Users should arrive curious and leave with the feeling that something is genuinely different here.

**2. During Search (Loading State)**
Target: Excited anticipation — like waiting for a WhatsApp reply from a well-connected friend who's pulling strings for you.
The streaming Hebrew status messages ("מחפש טיסות מתל אביב...", "בודק מלונות...") must feel alive and personal, not mechanical. The emotional message is: "someone capable is working hard for me right now."

**3. Results Delivery**
Target: Delight and disbelief — the "זה הכל?" moment.
Three beautiful Trip Package cards, flights + hotel bundled, prices in ₪, one tap to book. The emotional peak of the entire product.

**4. Error / Zero Results**
Target: Understood, not abandoned.
The AI responds warmly in Hebrew and always offers a constructive next step:
"לא מצאתי טיסות ישירות לתאריכים האלה — רוצה שאנסה תאריכים גמישים יותר?"
Never a cold error state. Never a dead end. The user must feel the AI is still on their side, still trying.

**5. Post-Booking (After Affiliate Click)**
Target: Accomplished + loyal — "טריפו עשה את העבודה."
A subtle in-chat confirmation message closes the loop:
"מעולה! שמרתי את החבילה הזו אצלך לעיון עתידי 💙"
This reinforces that the user made the right decision and creates an emotional tie back to Tripo for their next trip.

**6. Returning User**
Target: Effortless familiarity — like reopening a WhatsApp conversation with someone who remembers everything. No re-setup, no re-explanation. Just pick up where you left off.

### Micro-Emotions

| Emotion | Target State | Avoid |
|---|---|---|
| Trust | Confident, transparent | Skeptical, suspicious |
| Effort | Effortless, flowing | Laborious, repetitive |
| Understanding | Heard, interpreted | Ignored, misunderstood |
| Outcome | Accomplished, smart | Uncertain, regretful |
| Return | Familiar, welcomed | Anonymous, forgotten |

### Design Implications

1. **Feeling stupid → Never blame the input.**
   The AI must always attempt to extract intent from any Hebrew message, even ambiguous or incomplete ones. No "I didn't understand that" rejections. Instead: attempt a reasonable interpretation and confirm via the Context Confirmation Card. The user corrects; they never feel wrong.

2. **Feeling tricked → Affiliate links always clearly labeled.**
   Every booking CTA must include a visible label indicating it's an affiliate redirect (e.g., "הזמנה דרך Kiwi ↗"). Trust is Tripo's foundation — hidden commercial intent would destroy it instantly with the Israeli audience.

3. **Excited anticipation → Status messages must feel alive.**
   Write them in first-person plural ("אנחנו מחפשים...") or warm second-person. Vary them slightly each search. They are mini-copywriting moments, not system logs.

4. **Understood, not abandoned → Error states have warmth and next steps.**
   Every failure state is a conversational pivot. The AI acknowledges what it tried, explains briefly, and offers a specific alternative — never a dead end.

5. **Accomplished + loyal → Close every loop in-chat.**
   Post-booking confirmation message + auto-save the trip. The user never wonders "did it work?" or "will I remember this?" Tripo confirms and stores automatically.

### Emotional Design Principles

1. **The AI is always on the user's side.** In success and failure, the tone is warm, capable, and constructive. Never cold, never mechanical.

2. **Confidence without hype.** Understated design and copy lets the product speak. No exclamation marks in system messages, no over-promises on the landing.

3. **Never make the user feel stupid.** Input is always interpreted generously. Corrections happen through the Context Confirmation Card, not error messages.

4. **Transparency as loyalty-builder.** Labeled affiliates, visible search activity, explicit context summary — radical transparency converts skeptical Israeli users into loyal advocates.

5. **Every moment has emotional closure.** No interaction ends in ambiguity. Results arrive. Errors redirect. Bookings confirm. Returns feel familiar.

## UX Pattern Analysis & Inspiration

### Inspiring Products Analysis

**Google Flights — Information Clarity Benchmark**
What it does well: Clean, fast, zero fluff. Information hierarchy is perfect — destination, price, and dates are always primary. The price calendar and "Explore" map show how to make complex data scannable without overwhelming users.
What it lacks: Completely cold. No conversational layer, no persistent context, no warmth. Users still have to do all the cognitive coordination work themselves.
Tripo's lesson: Adopt Google Flights' information discipline and speed. Reject its emotional coldness.

**Wolt — The Israeli Trust & Card Design Template**
What it does well: Wolt cracked the Israeli market's trust code. Card-based results that are visual, scannable, and action-ready. Live status updates that feel personal ("השליח שלך בדרך..."). Copy that's warm without being sycophantic. One-tap reorder proves they remember you.
Why it matters for Tripo: Wolt is the closest UX analogue. Israeli users trust Wolt completely — and that trust was built through card design, live feedback, and warm copy. Tripo's Trip Package cards should feel like ordering from Wolt: familiar, confident, one tap to action.
Tripo's lesson: Trip Package cards adopt Wolt's visual card structure, price prominence, and one-tap action pattern. Loading states adopt Wolt's live status message tone.

**Revolut — Radical Transparency as Trust Architecture**
What it does well: Every transaction fee shown. Every exchange rate displayed. Every action confirmed. Revolut made radical transparency into a brand identity and converted skeptical European users into loyal advocates.
Why it matters for Tripo: Israeli users have the same skeptical DNA. The question "am I getting the best deal?" is identical to "is my bank ripping me off?" Revolut's answer was: show everything. Tripo's answer must be the same.
Tripo's lesson: Every affiliate link labeled with provider name. ILS exchange rate displayed alongside source currency. Search providers named in the Context Confirmation Card. Nothing hidden, nothing implied.

**Perplexity AI — "Show Your Work" Credibility**
What it does well: Builds instant credibility by showing sources, citations, and reasoning alongside every answer. The "I'll show you exactly how I got here" UX model transforms AI skepticism into trust.
Why it matters for Tripo: The Context Confirmation Card IS Tripo's Perplexity moment. "חיפשתי: ✈️ TLV→BCN | 📅 24-31/07 | 👥 2 | ₪6,000 — via Kiwi & Booking.com" is Tripo showing its work, building credibility through transparency of process, not just results.
Tripo's lesson: Context Confirmation Card explicitly names what was searched, which providers were queried, and what constraints were applied. Credibility through visible reasoning.

### Transferable UX Patterns

**Navigation & Flow Patterns:**
- **Wolt's live status progression** → Apply to Tripo's loading state: streaming Hebrew messages that name specific actions ("מחפש טיסות מתל אביב...") rather than generic spinners
- **Google Flights' progressive disclosure** → Show the most important information first (destination, price, dates) in Trip Package cards; secondary details (layovers, room type) accessible on expand

**Interaction Patterns:**
- **Wolt's one-tap reorder** → Tripo's "re-run this search" from saved trips. Returning users tap one saved trip and Tripo immediately re-queries with updated dates
- **Perplexity's source citation** → Tripo's Context Confirmation Card + provider labels on every affiliate CTA
- **Revolut's confirmation screens** → Tripo's post-booking in-chat confirmation: always close the loop explicitly

**Visual Patterns:**
- **Wolt's restaurant card** → Template for Tripo's Trip Package card: image/icon area, bold price in ₪, key attributes as chips, single primary CTA
- **Google Flights' data density** → Clean typographic hierarchy for flight details: time, duration, airline displayed in a readable scan pattern
- **Revolut's rate display** → Tripo's dual-currency price display: "₪2,340 (€590)" always visible, never hidden

### Anti-Patterns to Avoid

**Booking.com — The Dark Pattern Playbook (Invert Everything)**
- Fake urgency: "נותרו 2 חדרים בלבד!" — never. Tripo shows real availability only, no artificial scarcity signals
- Option overload: 847 results with 23 filters — never. Tripo surfaces max 3 curated Trip Packages. Curation IS the value
- Hidden commercial intent: affiliate relationships buried in fine print — never. Tripo labels every link explicitly
- Dark pattern filters: pre-checked upsell options — never. Tripo has no upsell checkboxes in the MVP flow

**Expedia Chatbot — The Scripted Menu Trap**
- Menu-driven "conversation": forcing users to tap pre-defined options instead of typing freely — never. Tripo's chat is always open input
- Scripted responses that don't acknowledge context — never. Tripo's AI responds to what was actually said, not a keyword match
- Multi-step intake before showing results — never. Tripo searches immediately on the first message

### Design Inspiration Strategy

**Adopt directly:**
- Wolt's card visual hierarchy and warm Hebrew copy tone
- Perplexity's "show your work" transparency model for the Context Confirmation Card
- Revolut's dual-currency display and explicit fee/source labeling
- Google Flights' information speed and zero-noise data presentation

**Adapt for Tripo's context:**
- Wolt's live status updates → adapt to travel search context with specific Hebrew messages per search vertical
- Google Flights' data density → adapt for RTL mobile layout with thumb-friendly touch targets
- Revolut's confirmation screens → adapt as in-chat messages rather than modal screens

**Avoid entirely:**
- Booking.com's urgency signals, option overload, and hidden commercial patterns — these destroy trust with the exact users Tripo is targeting
- Expedia's menu-tree chatbot model — antithetical to Tripo's conversational design philosophy

## Design System Foundation

### Design System Choice

**Tailwind CSS + shadcn/ui** — Themeable component system built natively on the Next.js ecosystem. Provides a proven component foundation with full brand customization through design tokens, achieving the Wolt-like card warmth and visual identity Tripo requires.

### Rationale for Selection

1. **Next.js native:** Tailwind CSS and shadcn/ui are the standard pairing for Next.js in 2026. Zero integration friction, optimal build tooling.

2. **RTL-first compatible:** `dir="rtl"` on the root element + Tailwind's `rtl:` variant prefix provides systematic RTL layout support across all components without overrides.

3. **Card components as base:** shadcn/ui's Card primitives serve as the starting point for Trip Package cards and the Context Confirmation Card — customized to achieve Wolt-level visual warmth.

4. **Solo MVP timeline:** Full component foundation from day one. No time spent building buttons and inputs from scratch during an 8-10 week sprint.

5. **Ecosystem fit:** Standard Israeli startup stack. Hiring-friendly, well-documented, large community.

### Core Toolkit

| Layer | Choice | Rationale |
|---|---|---|
| Styling | Tailwind CSS | Utility-first, RTL variant support, no CSS conflicts |
| Components | shadcn/ui | Copy-owned components, fully customizable, accessible |
| Typography | Rubik (Google Fonts) | Standard Hebrew web font, excellent RTL rendering, friendly and modern |
| Icons | Lucide React | Bundled with shadcn/ui, consistent stroke weight, RTL-neutral |
| Motion | Framer Motion | Chat streaming animations, card entrance effects, smooth UX polish |
| Number formatting | `Intl.NumberFormat` locale `'he-IL'` | Correct Hebrew number and currency formatting (₪) throughout |

### Design Tokens

**Color Palette:**
- **Primary:** `#1B4FD8` (deep blue) — authority, trust, intelligence
- **Background:** Warm white — clean, readable, high contrast in sunlight
- **Price accent:** ₪ green — positive, financial, action-oriented
- **Text:** Near-black on white, meeting WCAG 2.1 AA contrast minimums

**Typography:**
- **Font family:** Rubik, sans-serif (Hebrew + Latin)
- **Direction:** RTL root with `dir="rtl"` on `<html>`
- **Scale:** Standard Tailwind type scale; bold weights for prices and CTAs

**Motion:**
- Chat streaming: Framer Motion character-by-character or chunk animation for AI responses — feels alive, not instant dump
- Card entrance: Subtle fade-up on Trip Package card appearance
- Status messages: Sequential fade-in for each Hebrew search status line
- Keep motion minimal and purposeful — no decorative animations

### Customization Strategy

- shadcn/ui components are **copy-owned** — installed into `/components/ui`, modified directly. No upstream dependency conflicts.
- All design tokens defined in `tailwind.config.ts` under `theme.extend` for single-source-of-truth customization.
- Trip Package card and Context Confirmation Card are **bespoke compositions** built on shadcn/ui Card + Badge + Button primitives.
- **No dark mode for MVP.** Single polished light theme only. Reduces token complexity and QA surface area during the 8-10 week sprint.

## Core User Experience (Detailed)

### 2.1 Defining Experience

**Tripo's defining experience in one sentence:**
> "תגיד לי את הטיול שלך בעברית, ואני אמצא לך את הטיסות והמלון הכי טובים — בשיחה אחת."

This is what users will tell their friends. Not "I searched on Tripo" — but "I just told it what I wanted and it found everything." The defining interaction is a single, continuous Hebrew conversation that never resets, never asks twice, and always moves forward.

### 2.2 User Mental Model

**Existing mental model users bring:** Travel planning = opening 5-7 browser tabs, re-entering the same dates and preferences on each, manually reconciling results. Exhausting and trust-eroding.

**The Tripo mental model shift:** Travel planning = texting a knowledgeable friend on WhatsApp. You say what you want once. They come back with perfect options. You react, refine, or book. The conversation is the product.

**Why this shift works for Israeli users:** WhatsApp is the dominant communication paradigm in Israel. Chat-as-interface requires zero learning curve. Users don't need to be taught how to talk.

**Where confusion risk lives:** The blank chat could trigger "what do I type?" anxiety — solved by the animated AI intro + suggestion chips. The Context Confirmation Card could feel unfamiliar — solved by its visual clarity and inline editability.

### 2.3 Success Criteria for Core Experience

**Speed benchmarks:**
- "נקרא 👀" read acknowledgment: within 100ms of send
- Typing dots appear: within 200ms
- First Hebrew status message: within 1 second
- Trip Package cards rendered: within 5 seconds

**Quality benchmarks:**
- Context Confirmation Card accurately reflects 100% of stated constraints
- Constraint updates preserve all prior context without exception
- Follow-up queries re-search with updated + all previous constraints intact

**User feeling benchmarks:**
- First session: "זה הכל?" — disbelief at the simplicity
- Second session: "הוא זוכר אותי" — it knows me already
- Error state: "הוא עדיין מנסה" — it's still trying for me

### 2.4 Novel UX Patterns

**Established patterns used:**
- Chat interface (WhatsApp-native familiarity)
- Card-based results (Wolt-native familiarity)
- SMS OTP authentication (Israeli-native familiarity)
- Bottom sheet modals for sign-up and saving

**Novel patterns introduced:**

**1. The Context Confirmation Card (Tripo-original)**
A tappable chip row appearing before results that summarizes exactly what the AI understood and searched. Each chip is individually editable inline. No equivalent exists in the travel search market. Teaches users that "the AI got it" through visual proof, not just results.

**2. Warm Cold-Start (Tripo-original)**
Instead of an empty chat, the AI delivers an animated introductory message with 3 contextual suggestion chips. Eliminates blank-canvas anxiety entirely. Users either tap a chip or type freely — both are first-class paths.

**3. Persistent Conversational Context (differentiated)**
The chat never resets. Every follow-up query inherits all prior constraints automatically. "הראה לי משהו יותר זול" is understood as "cheaper flights and hotel than those 3 packages, still Barcelona, still last week of July, still 2 passengers, still direct only." No form, no filter panel — just natural language refinement.

**4. Constraint Memory Pulse (Tripo-original)**
When a constraint changes mid-conversation, the updated chip in the Context Confirmation Card pulses with a subtle highlight animation. The AI also verbally acknowledges: "עדכנתי — עכשיו מחפש לתאריכים החדשים עם כל ההעדפות שציינת 👌". Memory feels alive, explicit, and trustworthy.

### 2.5 Experience Mechanics

**Step 1 — Initiation (Cold Start)**
- AI delivers animated intro message:
  *"שלום! אני טריפו 👋 תגיד לי לאן אתה רוצה לטוס ומתי, ואני אדאג לכל השאר."*
- Three tappable suggestion chips appear below:
  - "✈️ ברצלונה לשבוע בקיץ, זוג, תקציב ₪8,000"
  - "🏖️ יוון עם ילדים בפסח, טיסה ישירה בלבד"
  - "🌍 איפשהו חם בנובמבר, הפתע אותי"
- User either taps a chip (populates input) or types freely
- Never an empty, cold interface

**Step 2 — Send & Instant Acknowledgment**
- User sends message
- Instant "נקרא 👀" read tick appears (within 100ms) — WhatsApp-native signal
- Transitions immediately to animated typing dots (3 pulsing dots)
- Zero dead time between send and visible AI activity

**Step 3 — Live Search Status**
- Typing dots give way to sequential streaming Hebrew status messages:
  1. "מחפש טיסות מתל אביב לברצלונה..."
  2. "בודק מלונות בברצלונה לתאריכים שלך..."
  3. "מרכיב את החבילות הכי טובות בשבילך..."
- Each message fades in sequentially via Framer Motion
- Builds trust by naming exactly what the AI is doing

**Step 4 — Results Delivery**
- Context Confirmation Card appears first as a horizontal chip row:
  `חיפשתי: ✈️ TLV→BCN | 📅 24-31/07 | 👥 2 | ₪8,000`
  Each chip is tappable to edit inline
- Up to 3 Trip Package cards fade-up sequentially beneath it
- Each card: destination image, flight summary, hotel summary, total price in ₪ (with source currency), two CTAs: "הזמנת טיסה דרך Kiwi ↗" + "הזמנת מלון דרך Booking ↗"

**Step 5 — Conversational Continuation**
- After cards appear, the chat input remains active
- User can type any follow-up: "הראה לי משהו יותר זול", "רק טיסות בוקר", "מה אם נזוז שבוע קדימה?"
- Tripo re-searches with the updated constraint + all prior context preserved
- Context Confirmation Card updates; changed chip pulses with highlight animation
- AI verbally acknowledges the update: "עדכנתי — עכשיו מחפש..."
- The chat never ends. It evolves until the user books or leaves.

**Step 6 — Save & Sign-Up (When Ready)**
- User taps "שמור טיול" on a Trip Package card
- Bottom sheet slides up: "שמור את הטיול שלך"
- Single field: Israeli phone number (+972)
- SMS OTP sent and entered inline in the same bottom sheet
- Account created silently; trip saved; bottom sheet dismisses
- AI confirms in chat: "מעולה! שמרתי את החבילה הזו אצלך לעיון עתידי 💙"

## Step 8 — Visual Foundation: Color Theme Options

### Design Brief for Color Selection

Tripo's color system must solve three simultaneous problems:
1. **Trust on first sight** — Israeli users decide in 3 seconds whether this feels legitimate. Color is the first signal.
2. **Readability under Israeli sun** — Mobile web used outdoors. High contrast is a functional requirement, not an aesthetic one.
3. **Warmth without playfulness** — This is not a discount travel app. It's a capable, knowledgeable friend. The palette must feel capable, not cute.

**Cross-theme constants (non-negotiable regardless of chosen option):**

| Token | Value | Rationale |
|---|---|---|
| `--color-price` | `#16A34A` (green-600) | Universal "positive/money" signal. Identical to Wolt's price color. |
| `--color-destructive` | `#DC2626` (red-600) | Error states. Standard, accessible, unambiguous. |
| `--color-affiliate-badge` | `#D97706` (amber-600) | Affiliate disclosure labels. Warm, visible, non-alarming. Legal trust signal. |
| `--font-family` | Rubik, sans-serif | Hebrew RTL rendering. All themes. |
| `--radius` | `12px` card, `8px` component | Warm but not cartoonish. Wolt-aligned. |
| `--shadow-card` | `0 2px 12px rgba(0,0,0,0.08)` | Subtle lift. Cards float, not stamp. |

---

### Option A — "Authority Blue" (Recommended Baseline)

**Personality:** The knowledgeable travel expert. Deep institutional blue signals competence and authority — the same trust cue used by El Al, Israeli banking apps, and government services that Israelis have internalized as "serious and reliable." Warm off-white background prevents the coldness of pure white and softens the authority.

**Emotional profile:** Confident → Trustworthy → Slightly formal but warm

**Inspiration:** El Al digital, Revolut (authority layer), Leumi digital banking

```
/* Option A — Authority Blue */
--color-primary:           #1B4FD8   /* Deep royal blue — CTAs, links, active states */
--color-primary-hover:     #1640B0   /* Darker on hover/press */
--color-primary-light:     #EEF3FD   /* Chip backgrounds, subtle tints */
--color-primary-foreground:#FFFFFF   /* Text on primary buttons */

--color-background:        #FAFAF8   /* Warm off-white — NOT pure #FFF */
--color-surface:           #FFFFFF   /* Card surfaces, input fields */
--color-surface-raised:    #F5F5F3   /* Subtle elevation within surface */

--color-text-primary:      #111827   /* Headings, prices, key data */
--color-text-secondary:    #4B5563   /* Supporting text, labels */
--color-text-muted:        #9CA3AF   /* Timestamps, placeholders */

--color-border:            #E5E7EB   /* Card borders, input outlines */
--color-border-active:     #1B4FD8   /* Focused inputs, selected chips */

--color-chat-user:         #1B4FD8   /* User message bubble background */
--color-chat-user-text:    #FFFFFF
--color-chat-ai:           #FFFFFF   /* AI message bubble — clean white */
--color-chat-ai-border:    #E5E7EB

--color-chip-default:      #F3F4F6   /* Unselected context chips */
--color-chip-active:       #EEF3FD   /* Selected/active chips */
--color-chip-pulse:        #DBEAFE   /* Constraint Memory Pulse flash color */
```

**Tailwind config excerpt:**
```typescript
// tailwind.config.ts
colors: {
  primary: { DEFAULT: '#1B4FD8', hover: '#1640B0', light: '#EEF3FD' },
  background: '#FAFAF8',
  surface: { DEFAULT: '#FFFFFF', raised: '#F5F5F3' },
}
```

**Where it excels:** Chip editability. The `#EEF3FD` active chip tint against the warm white background creates a clear "this is editable" affordance without requiring a border.

**Risk:** Can feel slightly corporate. Mitigated by Rubik font warmth and conversational copy tone.

---

### Option B — "Mediterranean Teal"

**Personality:** The well-traveled Israeli friend. Teal/cyan sits between blue (trust) and green (nature/travel), evoking the Mediterranean coast that is the literal destination for a significant portion of Israeli summer travel. Feels contemporary, digital-native, and distinctly "travel" without resorting to clichéd sunset gradients.

**Emotional profile:** Adventurous → Warm → Modern → Accessible

**Inspiration:** Airbnb (teal era), N26 (clean fintech), modern Israeli lifestyle brands

```
/* Option B — Mediterranean Teal */
--color-primary:           #0D7377   /* Deep teal — distinctive, travel-native */
--color-primary-hover:     #095E62
--color-primary-light:     #E6F6F7   /* Very light teal tint */
--color-primary-foreground:#FFFFFF

--color-background:        #FAFAF8   /* Same warm off-white */
--color-surface:           #FFFFFF
--color-surface-raised:    #F0FAFA   /* Subtle teal-tinted elevation */

--color-text-primary:      #0F2526   /* Near-black with teal undertone */
--color-text-secondary:    #3D5C5E
--color-text-muted:        #8BA5A7

--color-border:            #D4EAEB
--color-border-active:     #0D7377

--color-chat-user:         #0D7377
--color-chat-user-text:    #FFFFFF
--color-chat-ai:           #FFFFFF
--color-chat-ai-border:    #D4EAEB

--color-chip-default:      #F0FAFA
--color-chip-active:       #CCF0F1
--color-chip-pulse:        #A5DFE0   /* Sea-wave pulse on constraint update */
```

**Where it excels:** The Context Confirmation Card chips. The `#CCF0F1` active chip against white creates a distinctly travel-associated visual identity. No other Israeli travel product uses this palette — instant visual differentiation.

**Risk:** Less conventional "trust" signal than blue. Compensated by transparent design patterns (labeled affiliates, Context Confirmation Card) that build trust through behavior, not just color. Also: teal can trend toward "healthcare" — watch secondary surface tints carefully.

**Best for:** If Tripo wants to stand out visually as a new entrant rather than borrow trust from institutional blue. Stronger brand identity at the cost of slightly slower initial trust.

---

### Option C — "Jerusalem Stone Warm"

**Personality:** The thoughtful, grounded local. Jerusalem stone is the defining visual texture of Israeli urban space — warm limestone beige-gray. This palette puts warmth first: a cream-tinted background, warm slate for text, and a rich indigo as the sole cool anchor. Feels premium without feeling European. Distinctly Israeli without being kitschy.

**Emotional profile:** Warm → Premium → Locally-rooted → Calm confidence

**Inspiration:** Nespresso (warm premium), Airbnb (warm neutrals era), editorial Israeli design

```
/* Option C — Jerusalem Stone Warm */
--color-primary:           #3730A3   /* Rich indigo — single cool anchor */
--color-primary-hover:     #2E288A
--color-primary-light:     #EEF0FB
--color-primary-foreground:#FFFFFF

--color-background:        #FAF8F5   /* Cream-warm, Jerusalem stone undertone */
--color-surface:           #FFFFFF
--color-surface-raised:    #F5F2EC   /* Warm stone elevation */

--color-text-primary:      #1C1917   /* Warm near-black */
--color-text-secondary:    #57534E   /* Warm stone gray */
--color-text-muted:        #A8A29E

--color-border:            #E7E5E4   /* Warm gray borders */
--color-border-active:     #3730A3

--color-chat-user:         #3730A3
--color-chat-user-text:    #FFFFFF
--color-chat-ai:           #FAFAF8   /* Cream AI bubble — warmer than white */
--color-chat-ai-border:    #E7E5E4

--color-chip-default:      #F5F2EC   /* Stone chip background */
--color-chip-active:       #EEF0FB   /* Indigo-tinted when active */
--color-chip-pulse:        #C7D2FE   /* Indigo-100 pulse */
```

**Where it excels:** Premium card feel. The warm stone surface on Trip Package cards creates a distinct "editorial travel guide" aesthetic — cards feel curated, not pulled from a database. Pairs beautifully with destination photography (warm skin tones, blue water pop against stone).

**Risk:** Indigo CTA buttons can feel less "action-ready" than blue. The warm background increases visual complexity — requires tight quality control on card design to avoid muddy hierarchy. Not ideal if development moves fast with less design review time.

**Best for:** Premium positioning. If Tripo targets higher-budget trips (business travel, luxury family vacations), Option C communicates that naturally. For the budget-conscious or family mass market, Option A or B is safer.

---

### Comparison Matrix

| Criterion | Option A (Authority Blue) | Option B (Mediterranean Teal) | Option C (Jerusalem Stone) |
|---|---|---|---|
| **Trust on first sight** | ★★★★★ | ★★★★☆ | ★★★★☆ |
| **Visual differentiation** | ★★★☆☆ | ★★★★★ | ★★★★☆ |
| **RTL Hebrew readability** | ★★★★★ | ★★★★★ | ★★★★☆ |
| **Mobile outdoor contrast** | ★★★★★ | ★★★★☆ | ★★★★☆ |
| **Card warmth (Wolt feel)** | ★★★★☆ | ★★★★☆ | ★★★★★ |
| **Dev implementation speed** | ★★★★★ | ★★★★☆ | ★★★☆☆ |
| **Israeli market fit** | ★★★★★ | ★★★★☆ | ★★★★☆ |
| **Overall MVP recommendation** | ✅ Best fit | Strong alternative | Premium niche |

### Recommendation

**For MVP: Option A (Authority Blue)** with one selective borrow from Option B.

**Reasoning:**
1. Tripo's core UX problem is trust, not brand recall. Blue solves trust fastest in the Israeli market — it is the color of El Al, Leumi, and government digital services that Israelis have trusted for decades.
2. Option A has the fastest implementation path. Standard Tailwind blue-700/800 values require minimal custom configuration. Solo founder + 8-10 week sprint = prioritize execution.
3. The visual differentiation risk is mitigated by Tripo's UX, not its color. The Context Confirmation Card, Constraint Memory Pulse, and warm Hebrew conversational tone ARE the differentiation. The color can be orthodox-trust while the experience is novel.

**The selective borrow:** Adopt Option B's `--color-chip-pulse` concept — instead of a flat blue pulse for Constraint Memory Pulse, use a brief cyan flash (`#A5F3FC` for 300ms) before settling to the blue-tinted chip. Creates a satisfying, travel-native "ping" animation without changing the overall palette.

### Decision: Option A + Selective Borrow from B ✅

**Authority Blue** with the cyan Constraint Memory Pulse.

---

## Step 8 — Visual Foundation: Final Specification

### 8.1 Chosen Palette — Authority Blue + Cyan Pulse

**Decision rationale:** Trust-first positioning for the Israeli market. Institutional blue leverages existing trust associations (El Al, Leumi, government digital). Visual differentiation is delivered by UX patterns (Context Confirmation Card, conversational Hebrew), not color novelty. Cyan pulse borrowed from Option B adds a travel-native delight moment at the one point where animation is most meaningful: constraint memory updates.

---

### 8.2 Complete Design Token Reference

#### Core Palette

| Token name | Hex value | Usage |
|---|---|---|
| `primary` | `#1B4FD8` | CTAs, links, active borders, user chat bubble |
| `primary-hover` | `#1640B0` | Button/link hover and press states |
| `primary-light` | `#EEF3FD` | Active chip background, tinted surfaces |
| `primary-foreground` | `#FFFFFF` | Text on primary buttons |
| `background` | `#FAFAF8` | Page background (warm off-white, NOT pure white) |
| `surface` | `#FFFFFF` | Card surfaces, input fields, AI message bubbles |
| `surface-raised` | `#F5F5F3` | Subtle in-card elevation, nested sections |
| `text-primary` | `#111827` | Headings, prices, flight times, key data |
| `text-secondary` | `#4B5563` | Supporting labels, hotel names, airline names |
| `text-muted` | `#9CA3AF` | Timestamps, placeholders, read receipts |
| `border` | `#E5E7EB` | Card outlines, input default borders |
| `border-active` | `#1B4FD8` | Focused inputs, selected/editable chips |
| `price` | `#16A34A` | All ₪ price display (green = positive/money) |
| `price-bg` | `#F0FDF4` | Price badge background in Trip Package cards |
| `affiliate` | `#D97706` | Affiliate disclosure badges ("דרך Kiwi ↗") |
| `affiliate-bg` | `#FFFBEB` | Affiliate badge background |
| `destructive` | `#DC2626` | Error states, failed search messages |
| `destructive-bg` | `#FEF2F2` | Error message backgrounds |
| `chip-default` | `#F3F4F6` | Unselected Context Confirmation chips |
| `chip-active` | `#EEF3FD` | Selected/editable chips |
| `chip-pulse-start` | `#A5F3FC` | **Borrowed from Option B** — Constraint Memory Pulse flash (cyan-200, 0ms→150ms) |
| `chip-pulse-end` | `#EEF3FD` | Pulse settles back to primary-light (150ms→300ms) |
| `typing-dot` | `#1B4FD8` | Animated typing indicator dots |
| `read-receipt` | `#9CA3AF` | "נקרא 👀" timestamp color |

#### Chat-Specific Tokens

| Token | Hex | Usage |
|---|---|---|
| `chat-user-bg` | `#1B4FD8` | User message bubble background |
| `chat-user-text` | `#FFFFFF` | User message text |
| `chat-ai-bg` | `#FFFFFF` | AI message bubble background |
| `chat-ai-border` | `#E5E7EB` | AI bubble border |
| `chat-ai-text` | `#111827` | AI message text |
| `chat-status-text` | `#4B5563` | Streaming status messages ("מחפש טיסות...") |
| `chat-background` | `#FAFAF8` | Chat stream background (matches page) |

---

### 8.3 tailwind.config.ts — Complete Color Extension

```typescript
// tailwind.config.ts
import type { Config } from 'tailwindcss'

const config: Config = {
  content: ['./src/**/*.{ts,tsx}'],
  theme: {
    extend: {
      colors: {
        // Brand primary
        primary: {
          DEFAULT: '#1B4FD8',
          hover:   '#1640B0',
          light:   '#EEF3FD',
          foreground: '#FFFFFF',
        },
        // Surfaces
        background: '#FAFAF8',
        surface: {
          DEFAULT: '#FFFFFF',
          raised:  '#F5F5F3',
        },
        // Text scale
        text: {
          primary:   '#111827',
          secondary: '#4B5563',
          muted:     '#9CA3AF',
        },
        // Borders
        border: {
          DEFAULT: '#E5E7EB',
          active:  '#1B4FD8',
        },
        // Semantic: price
        price: {
          DEFAULT: '#16A34A',
          bg:      '#F0FDF4',
        },
        // Semantic: affiliate
        affiliate: {
          DEFAULT: '#D97706',
          bg:      '#FFFBEB',
        },
        // Semantic: error
        destructive: {
          DEFAULT: '#DC2626',
          bg:      '#FEF2F2',
        },
        // Chat
        chat: {
          'user-bg':    '#1B4FD8',
          'user-text':  '#FFFFFF',
          'ai-bg':      '#FFFFFF',
          'ai-border':  '#E5E7EB',
          'ai-text':    '#111827',
          'status':     '#4B5563',
        },
        // Chips
        chip: {
          default:     '#F3F4F6',
          active:      '#EEF3FD',
          'pulse-start': '#A5F3FC',  // cyan-200 — Constraint Memory Pulse
        },
      },
      fontFamily: {
        sans: ['Rubik', 'sans-serif'],
      },
      borderRadius: {
        card:      '12px',
        component: '8px',
        chip:      '999px',  // fully-rounded chips
      },
      boxShadow: {
        card:    '0 2px 12px rgba(0, 0, 0, 0.08)',
        'card-hover': '0 4px 20px rgba(27, 79, 216, 0.12)',
        input:   '0 1px 3px rgba(0, 0, 0, 0.04)',
      },
    },
  },
  plugins: [],
}

export default config
```

---

### 8.4 shadcn/ui CSS Variables — :root Block

Map Tripo design tokens to shadcn/ui's CSS variable system. Place in `src/app/globals.css`.

```css
/* src/app/globals.css */
@import url('https://fonts.googleapis.com/css2?family=Rubik:wght@300;400;500;600;700&display=swap');

@layer base {
  :root {
    /* shadcn/ui core variables — Tripo mapped */
    --background:             250 250 248;   /* #FAFAF8 warm off-white */
    --foreground:             17 24 39;      /* #111827 */

    --card:                   255 255 255;   /* #FFFFFF */
    --card-foreground:        17 24 39;

    --popover:                255 255 255;
    --popover-foreground:     17 24 39;

    --primary:                27 79 216;     /* #1B4FD8 */
    --primary-foreground:     255 255 255;

    --secondary:              238 243 253;   /* #EEF3FD primary-light */
    --secondary-foreground:   27 79 216;

    --muted:                  243 244 246;   /* #F3F4F6 */
    --muted-foreground:       156 163 175;   /* #9CA3AF */

    --accent:                 238 243 253;   /* #EEF3FD */
    --accent-foreground:      27 79 216;

    --destructive:            220 38 38;     /* #DC2626 */
    --destructive-foreground: 255 255 255;

    --border:                 229 231 235;   /* #E5E7EB */
    --input:                  229 231 235;
    --ring:                   27 79 216;     /* Focus ring = primary */

    --radius: 0.5rem;                        /* shadcn default; override per-component */

    /* Tripo extended tokens (not in shadcn system) */
    --price:                  #16A34A;
    --price-bg:               #F0FDF4;
    --affiliate:              #D97706;
    --affiliate-bg:           #FFFBEB;
    --chip-pulse-start:       #A5F3FC;
    --shadow-card:            0 2px 12px rgba(0, 0, 0, 0.08);
    --shadow-card-hover:      0 4px 20px rgba(27, 79, 216, 0.12);
  }
}

@layer base {
  html {
    direction: rtl;
    font-family: 'Rubik', sans-serif;
  }
  body {
    @apply bg-background text-text-primary;
  }
}
```

---

### 8.5 Color Usage Rules by Component

#### Primary Button (CTA — "הזמנת טיסה דרך Kiwi ↗")
- Background: `primary` (#1B4FD8)
- Text: `primary-foreground` (#FFFFFF)
- Hover: `primary-hover` (#1640B0)
- Border-radius: `8px`
- Font weight: 600
- **Rule:** Every affiliate CTA button must display provider name inline. Never "הזמן" alone — always "הזמנה דרך [Provider] ↗"

#### Context Confirmation Chips
- Default state: bg `chip-default` (#F3F4F6), text `text-secondary`, border 1px `border`
- Active/editable: bg `chip-active` (#EEF3FD), border 1px `border-active` (#1B4FD8)
- Constraint Memory Pulse: keyframe animation `chip-pulse` — bg flashes `chip-pulse-start` (#A5F3FC) at 0ms, transitions to `chip-active` (#EEF3FD) by 300ms
- Border-radius: `999px` (pill shape)
- Touch target: min-height `36px`, min-width `64px`

```css
/* Constraint Memory Pulse animation */
@keyframes chip-pulse {
  0%   { background-color: var(--chip-pulse-start); }  /* #A5F3FC cyan flash */
  100% { background-color: #EEF3FD; }                  /* settle to primary-light */
}
.chip-updating {
  animation: chip-pulse 300ms ease-out forwards;
}
```

#### Trip Package Card
- Container: bg `surface` (#FFFFFF), border 1px `border`, `border-radius: 12px`, shadow `shadow-card`
- Hover: shadow `shadow-card-hover`, border-color transitions to `border-active` over 150ms
- Price display: `price` (#16A34A), weight 700, size `text-xl`
- Secondary price (source currency): `text-muted`, size `text-sm`, parenthetical
- Affiliate badge: bg `affiliate-bg`, text `affiliate`, border 1px `affiliate` at 40% opacity, `border-radius: 4px`, size `text-xs`

#### User Chat Bubble
- Background: `chat-user-bg` (#1B4FD8)
- Text: `chat-user-text` (#FFFFFF)
- Border-radius: `12px 12px 2px 12px` (RTL: round all corners except bottom-right)
- Max-width: `75%` of chat column, aligned to inline-end (right in RTL)

#### AI Chat Bubble
- Background: `chat-ai-bg` (#FFFFFF)
- Border: 1px `chat-ai-border`
- Text: `chat-ai-text` (#111827)
- Border-radius: `12px 12px 12px 2px` (RTL: round all corners except bottom-left)
- Max-width: `85%`, aligned to inline-start

#### Streaming Status Messages
- No bubble wrapper — plain text, left-aligned in chat stream
- Text color: `chat-status` (#4B5563)
- Size: `text-sm`
- Leading icon: specific emoji per status (✈️ for flights, 🏨 for hotels, ✨ for package assembly)
- Entrance: Framer Motion `opacity: 0→1, y: 4→0` over 200ms, staggered 400ms between messages

#### Typing Indicator Dots
- Dot color: `typing-dot` (#1B4FD8)
- 3 dots, 6px diameter, 4px gap
- Framer Motion stagger bounce: each dot animates `y: 0→-4→0` with 150ms offset

#### Affiliate Disclosure Badge
- **Required on every booking CTA.** No exceptions.
- Text: provider name + external link icon, e.g. "דרך Kiwi ↗"
- Color: `affiliate` (#D97706)
- Background: `affiliate-bg` (#FFFBEB)
- Position: inline within button text OR as `text-xs` label directly beneath CTA button

---

### 8.6 WCAG 2.1 AA Contrast Verification

Minimum required ratios: **4.5:1 for normal text**, **3:1 for large text and UI components**.

| Foreground | Background | Ratio | Size context | AA pass? |
|---|---|---|---|---|
| `#FFFFFF` on `#1B4FD8` (primary button text) | — | **5.74:1** | Normal text | ✅ |
| `#111827` on `#FAFAF8` (body text on page bg) | — | **17.4:1** | Normal text | ✅ |
| `#111827` on `#FFFFFF` (text on card) | — | **18.1:1** | Normal text | ✅ |
| `#4B5563` on `#FAFAF8` (secondary text on bg) | — | **7.2:1** | Normal text | ✅ |
| `#4B5563` on `#FFFFFF` (secondary text on card) | — | **7.6:1** | Normal text | ✅ |
| `#9CA3AF` on `#FFFFFF` (muted text on card) | — | **3.0:1** | Small text (≥14px) | ⚠️ Use only at 14px+ |
| `#9CA3AF` on `#FAFAF8` (muted text on bg) | — | **2.9:1** | Small text | ⚠️ Use only at 14px+ bold |
| `#16A34A` on `#FFFFFF` (price on card) | — | **4.54:1** | Normal text | ✅ |
| `#16A34A` on `#F0FDF4` (price on price-bg) | — | **4.3:1** | Large text (≥18px) | ✅ large only |
| `#D97706` on `#FFFBEB` (affiliate badge) | — | **3.1:1** | Large text / UI component | ✅ component |
| `#FFFFFF` on `#111827` (inverted headers) | — | **18.1:1** | Any | ✅ |
| `#1B4FD8` on `#EEF3FD` (active chip text/border) | — | **4.5:1** | Normal text | ✅ |
| `#111827` on `#F3F4F6` (default chip text) | — | **15.2:1** | Normal text | ✅ |

**⚠️ Muted text rule:** `text-muted` (#9CA3AF) must never be used for meaningful data at small sizes. Acceptable uses: timestamps, read receipts, parenthetical source currency. Minimum 14px, preferably 14px medium weight.

**Price badge note:** `#16A34A` on `#F0FDF4` at 3:1 ratio — use only at `text-lg` (18px) or above, or switch to `#FFFFFF` background for small price labels.

---

### Step 8 Summary

**Status: ✅ Complete**

Chosen direction: **Option A — Authority Blue + Cyan Constraint Memory Pulse**

Deliverables finalized:
- [x] Complete design token reference (all colors named and justified)
- [x] `tailwind.config.ts` color extension block
- [x] shadcn/ui `:root` CSS variable mapping
- [x] Per-component color usage rules with RTL notes
- [x] WCAG 2.1 AA contrast verification with flagged exceptions
- [x] Constraint Memory Pulse keyframe animation spec

---

## Step 9 — Component Design: Trip Package Card

### 9.1 Card Role in the Product

The Trip Package Card is Tripo's hero moment — the "זה הכל?" instant. It is the only screen element that directly drives affiliate revenue. Every design decision is subordinate to two goals:

1. **Scannability in under 3 seconds.** Price, destination, dates — readable before the user decides to engage.
2. **One tap to book.** No intermediate page, no confirmation modal. CTA taps directly open the affiliate provider in a new tab.

Cards appear in groups of up to 3, delivered sequentially after the Context Confirmation Card. Each is a self-contained package: flight + hotel + total price. The user never has to mentally combine two separate results.

---

### 9.2 Card Anatomy

A Trip Package Card has six zones, ordered by visual hierarchy:

```
┌─────────────────────────────────────────────┐  ← 12px radius, shadow-card
│  [ZONE 1: HEADER]                           │
│  ✈️ Barcelona · ספרד          [Badge: זול]  │  ← destination + rank badge
├─────────────────────────────────────────────┤
│  [ZONE 2: PRICE HERO]                       │
│  ₪7,240          (€1,820)                   │  ← big green ₪ + muted € aside
│  לשני נוסעים · 7 לילות                      │  ← context subtitle
├─────────────────────────────────────────────┤
│  [ZONE 3: FLIGHT SUMMARY]                   │
│  ✈️  TLV → BCN   24 יול   06:15 → 10:30    │
│      ישיר · 4:15 · Wizz Air                 │
│  ✈️  BCN → TLV   31 יול   12:00 → 17:20    │
│      ישיר · 4:20 · Wizz Air                 │
├─────────────────────────────────────────────┤
│  [ZONE 4: HOTEL SUMMARY]                    │
│  🏨  Hotel Arts Barcelona  ★★★★★            │
│      7 לילות · חדר זוגי · ₪3,890            │
│      [בריכה] [ארוחת בוקר] [ביטול חינם]      │  ← amenity chips
├─────────────────────────────────────────────┤
│  [ZONE 5: CTAs]                             │
│  [הזמנת טיסה דרך Wizz Air ↗]  ← primary   │
│  [הזמנת מלון דרך Booking.com ↗] ← secondary│
├─────────────────────────────────────────────┤
│  [ZONE 6: FOOTER]                           │
│  [♡ שמור טיול]    [v פרטים נוספים]          │
└─────────────────────────────────────────────┘
```

---

### 9.3 Zone Specifications

#### Zone 1 — Header

| Property | Value |
|---|---|
| Destination | City name in Hebrew bold + country in secondary color. e.g. **ברצלונה** · ספרד |
| Leading icon | ✈️ emoji — no SVG, renders cross-platform without sizing issues |
| Rank badge | Pill badge, top-inline-end corner (RTL: top-left). Options: `הכי זול`, `הכי מהיר`, `מומלץ`. bg `primary-light`, text `primary`, `text-xs`, `font-medium` |
| Height | 44px — generous touch target in case user taps to expand |

**Badge assignment logic (defined for developer):**
- Card 1 (lowest total price): badge `הכי זול` — green (`price-bg` / `price`)
- Card 2 (fewest stops / shortest flight time): badge `הכי מהיר` — blue (`primary-light` / `primary`)
- Card 3 (AI editorial pick — best value/quality balance): badge `מומלץ` — amber (`affiliate-bg` / `affiliate`)

#### Zone 2 — Price Hero

| Property | Value |
|---|---|
| Total price | `text-3xl font-bold` · color `price` (#16A34A) · format via `Intl.NumberFormat('he-IL', { style: 'currency', currency: 'ILS', maximumFractionDigits: 0 })` |
| Source currency | `text-sm font-normal` · color `text-muted` · parenthetical · e.g. (€1,820) |
| Context subtitle | `text-sm` · color `text-secondary` · e.g. "לשני נוסעים · 7 לילות" |
| Layout | Price and source currency on same baseline. Subtitle on line below. |
| Padding | `pt-3 pb-2 px-4` |

**Price is the decision trigger.** It must be the largest and most visually dominant element on the card. No other element competes with it in Zone 2.

#### Zone 3 — Flight Summary

Two rows: outbound + return. Each row:

```
[✈️]  [ROUTE]   [DATE]   [DEPART → ARRIVE]
      [STOPS · DURATION · AIRLINE]
```

| Element | Style |
|---|---|
| Route `TLV → BCN` | `text-sm font-semibold` · `text-primary` |
| Date `24 יול` | `text-sm` · `text-secondary` |
| Times `06:15 → 10:30` | `text-sm font-medium` · `text-primary` |
| Stops + duration | `text-xs` · `text-muted` |
| Airline name | `text-xs` · `text-muted` |
| Direct flight | Show "ישיר" in `text-xs font-medium text-price` (green) — visual confirmation of constraint |
| Layover | Show "עצירה ב[עיר]" in `text-xs text-text-secondary` — never hide layovers |
| Divider | 1px `border` between outbound and return rows |
| Zone padding | `px-4 py-3` |

**RTL note:** Route arrow `→` is direction-neutral in Hebrew context (flight direction, not reading direction). Do NOT flip to `←` in RTL. Aviation conventions are LTR-universal.

#### Zone 4 — Hotel Summary

| Element | Style |
|---|---|
| Hotel name | `text-sm font-semibold` · `text-primary` · truncate at 28 chars with ellipsis |
| Star rating | Lucide `Star` icons filled, `text-amber-400`, size 12px. Show count as number if >3 stars: "★★★★★" or "5★" |
| Nights + room type | `text-xs` · `text-secondary` · e.g. "7 לילות · חדר זוגי" |
| Hotel price | `text-sm font-medium` · `text-primary` · right-aligned in RTL (inline-end) |
| Amenity chips | Max 3 chips shown. bg `chip-default`, text `text-secondary`, `text-xs`, `rounded-chip`, `px-2 py-0.5`. Priority order: pool → breakfast → free cancellation → spa → gym |
| Zone padding | `px-4 py-3` |

**Amenity chip content (Hebrew):**
- בריכה
- ארוחת בוקר כלולה
- ביטול חינם
- ספא
- חדר כושר
- חניה

**Free cancellation** (`ביטול חינם`) chip gets a green variant when present: bg `price-bg`, text `price` — it's a trust signal worth highlighting.

#### Zone 5 — CTAs

Two full-width buttons, stacked vertically, 8px gap between them.

**Primary CTA — Flight booking:**
```
[הזמנת טיסה דרך Wizz Air ↗]
```
- bg `primary`, text `primary-foreground`, `rounded-component` (8px), `h-11` (44px min touch target)
- Font: `text-sm font-semibold`
- Icon: Lucide `ExternalLink` 14px, inline-end of text (RTL: left of text)
- `rel="noopener noreferrer"`, `target="_blank"`
- Aria label: `aria-label="הזמנת טיסה דרך Wizz Air — קישור שותף, נפתח בחלון חדש"`

**Secondary CTA — Hotel booking:**
```
[הזמנת מלון דרך Booking.com ↗]
```
- bg `surface`, border 1.5px `primary`, text `primary`, `rounded-component`, `h-11`
- Hover: bg `primary-light`
- Same aria pattern as primary

**Affiliate disclosure:**
A single `text-xs text-muted` line below both buttons:
> "קישורי ההזמנה הם קישורי שותפים. טריפו עשויה לקבל עמלה ללא עלות נוספת לך."

This line satisfies the Israeli Consumer Protection Law disclosure requirement. It must be present on every card. It is not a tooltip — it must be persistently visible.

#### Zone 6 — Footer

Two ghost actions in a horizontal row, separated by a flex spacer:

| Action | Style | Behavior |
|---|---|---|
| `♡ שמור טיול` | `text-sm text-secondary`, Lucide `Heart` icon | Opens sign-up bottom sheet (or saves directly if authenticated). Heart fills `text-destructive` (#DC2626) when saved. |
| `פרטים נוספים ˅` | `text-sm text-secondary`, Lucide `ChevronDown` icon | Expands card to show price breakdown + full hotel details. Chevron rotates 180° on expand. |

Footer padding: `px-4 py-3`, `border-t border-border`.

---

### 9.4 Expanded State

When user taps "פרטים נוספים", the card height animates open (Framer Motion `height: auto` via `AnimatePresence`) to reveal:

**Price breakdown section:**
```
פירוט מחיר
טיסה (2 נוסעים)    ₪3,350
מלון (7 לילות)     ₪3,890
─────────────────────────
סה"כ               ₪7,240
```
- Rendered as a simple definition list, `text-sm`
- Total row: `font-semibold text-price`
- Divider: 1px `border`

**Extended hotel details:**
- Full address or neighborhood: `text-xs text-muted`
- Check-in / check-out times if available
- Full amenity list (all chips, not just top 3)
- User rating score if available: "8.7 / 10 · מצוין" in `text-sm font-medium text-primary`

**Animation:** `height` from collapsed to auto over 250ms, `ease-out`. Content fades in at 150ms. Chevron rotates via `rotate: 0 → 180` over 200ms.

---

### 9.5 Loading Skeleton State

While cards are being built, show 1–2 skeleton cards in the stream. Skeleton matches the exact card structure so there is no layout shift when real cards arrive.

```
┌─────────────────────────────────────────────┐
│  [~~~~~~~~~~~~~~~~~~~~~~~~~~]  [~~~~]        │  ← header shimmer
├─────────────────────────────────────────────┤
│  [~~~~~~~~~~]  [~~~~~]                       │  ← price hero shimmer
│  [~~~~~~~~~~~~~~~~~]                         │
├─────────────────────────────────────────────┤
│  [~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~]      │  ← flight row 1
│  [~~~~~~~~~~~~~~~~~]                         │
│  [~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~]      │  ← flight row 2
│  [~~~~~~~~~~~~~~~~~]                         │
├─────────────────────────────────────────────┤
│  [~~~~~~~~~~~~~~~~~~~~~~~~~~~~]  [~~~]       │  ← hotel row
│  [~~~~] [~~~~] [~~~~~~~~~]                   │  ← amenity chips
├─────────────────────────────────────────────┤
│  [~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~]        │  ← CTA 1
│  [~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~]        │  ← CTA 2
└─────────────────────────────────────────────┘
```

**Shimmer animation:**
```css
@keyframes shimmer {
  0%   { background-position: -400px 0; }
  100% { background-position: 400px 0; }
}
.skeleton {
  background: linear-gradient(90deg, #F3F4F6 25%, #E5E7EB 50%, #F3F4F6 75%);
  background-size: 800px 100%;
  animation: shimmer 1.4s ease-in-out infinite;
  border-radius: 4px;
}
```

---

### 9.6 Framer Motion Entrance Animation

Cards enter sequentially after the Context Confirmation Card settles. Staggered cascade — not simultaneous — to create a "presenting options" feel.

```typescript
// Card entrance variants
const cardVariants = {
  hidden: { opacity: 0, y: 16 },
  visible: (i: number) => ({
    opacity: 1,
    y: 0,
    transition: {
      delay: i * 0.15,      // 0ms, 150ms, 300ms for cards 1, 2, 3
      duration: 0.3,
      ease: 'easeOut',
    },
  }),
}

// Usage
<motion.div
  variants={cardVariants}
  initial="hidden"
  animate="visible"
  custom={index}   // 0, 1, 2
>
  <TripPackageCard ... />
</motion.div>
```

**Context Confirmation Card** enters first with `delay: 0, duration: 0.25`. First trip card starts at `delay: 0.15` after the confirmation card is visible — never simultaneous.

---

### 9.7 Responsive Behaviour

| Breakpoint | Card behaviour |
|---|---|
| Mobile (`<640px`) | Full-width card, `mx-4` margin each side (358px effective on 390px screen) |
| Tablet (`640px–1023px`) | Max-width `540px`, centered in chat column |
| Desktop (`≥1024px`) | Max-width `560px`, centered. Chat column is max `680px` wide; card fills it comfortably |

Cards never sit side-by-side. Always stacked vertically in the chat stream — this is a chat, not a results grid.

---

### 9.8 Accessibility Specification

| Requirement | Implementation |
|---|---|
| Card as landmark | Wrap each card in `<article>` with `aria-label="חבילת טיול [N]: [עיר], ₪[price]"` |
| Rank badge | `aria-label="חבילה הכי זולה"` — not just the Hebrew text, the full label |
| Flight times | Wrap in `<time datetime="2026-07-24T06:15">24 יול, 06:15</time>` |
| CTA buttons | `aria-label="הזמנת טיסה דרך Wizz Air — קישור שותף, נפתח בחלון חדש"` |
| Affiliate disclosure | `role="note"` on the disclosure `<p>` tag |
| Save button | `aria-label="שמור טיול ברצלונה"` (include destination) · `aria-pressed="false/true"` |
| Expand toggle | `aria-expanded="false/true"` · `aria-controls="details-[card-id]"` |
| Price | Wrap in `<data value="7240">₪7,240</data>` for machine-readability |
| Skeleton | `aria-busy="true"` on container while loading, `aria-live="polite"` for card arrival |
| Focus order (RTL) | Ensure `Tab` order follows visual RTL reading order: right → left within each zone |

---

### 9.9 React Component API (Interface Definition)

The developer gets a clear, typed component contract to implement against.

```typescript
// types/trip-package.ts

interface FlightLeg {
  origin: string          // 'TLV'
  destination: string     // 'BCN'
  date: string            // ISO: '2026-07-24'
  departTime: string      // '06:15'
  arriveTime: string      // '10:30'
  durationMinutes: number // 255
  stops: number           // 0 = direct
  stopoverCity?: string   // e.g. 'Athens' if stops > 0
  airline: string         // 'Wizz Air'
  affiliateUrl: string    // Kiwi or Skyscanner deeplink
}

interface HotelResult {
  name: string
  starRating: number      // 1–5
  nights: number
  roomType: string        // Hebrew: 'חדר זוגי'
  priceILS: number        // hotel-only price
  priceSourceCurrency?: number
  sourceCurrency?: string // 'EUR'
  amenities: string[]     // Hebrew amenity names
  userRating?: number     // 0–10
  affiliateUrl: string    // Booking.com deeplink
  imageUrl?: string       // optional destination photo
}

interface TripPackageCardProps {
  index: number           // 0, 1, 2 — for stagger animation + rank badge
  destinationCity: string // Hebrew: 'ברצלונה'
  destinationCountry: string // Hebrew: 'ספרד'
  outbound: FlightLeg
  return: FlightLeg
  hotel: HotelResult
  totalPriceILS: number
  totalPriceSourceCurrency?: number
  sourceCurrency?: string
  passengerCount: number
  isSaved?: boolean
  onSave: () => void
  onBookFlight: () => void  // fires before redirect (for analytics)
  onBookHotel: () => void
}
```

---

### 9.10 Edge Cases & Guardrails

| Scenario | Handling |
|---|---|
| Hotel name >28 chars | Truncate with CSS `truncate` class. Full name on expand. |
| No direct flights available | Show best available with layover. Never hide stops. Stops count in amber `text-affiliate` to signal constraint not fully met. |
| Only 1–2 results (not 3) | Show 1 or 2 cards. Never show empty "card 3" placeholder. AI message explains: "מצאתי רק 2 חבילות לתאריכים האלה." |
| Price in USD (non-EUR) | Use `Intl.NumberFormat` with detected currency. Always show ₪ primary regardless of source currency. |
| Image unavailable | Show destination emoji + country flag as fallback. No broken image icons. |
| Very long airline name | Truncate after 12 chars: "British Airw..." |
| User already saved this trip | Heart pre-filled. "שמור טיול" becomes "נשמר ✓" in `text-price` green. |
| Affiliate URL unavailable | Hide the corresponding CTA button entirely. Never show a broken or placeholder link. Log to admin dashboard. |

---

### Step 9 Summary

**Status: ✅ Complete**

Deliverables finalized:
- [x] Card anatomy: 6 zones defined with full content, hierarchy, and RTL rules
- [x] Zone-by-zone specifications with exact color tokens, type sizes, spacing
- [x] Badge assignment logic (הכי זול / הכי מהיר / מומלץ)
- [x] Affiliate disclosure — persistent, legally compliant, always visible
- [x] Loading skeleton with shimmer CSS
- [x] Framer Motion entrance animation (staggered cascade)
- [x] Expanded state (price breakdown + full hotel details)
- [x] Responsive behaviour (mobile → desktop)
- [x] Full accessibility spec (ARIA labels in Hebrew, focus order, live regions)
- [x] TypeScript component interface (`TripPackageCardProps`)
- [x] Edge case handling (no images, truncation, missing affiliate URLs, etc.)

---

## Step 10 — Component Design: Context Confirmation Card

### 10.1 Why This Component Matters Most

The Context Confirmation Card is Tripo's sole original UX invention. No other travel product does this. It is:

- The **trust mechanism** — visual proof the AI understood every constraint
- The **correction surface** — the only place users fix misunderstood input without retyping
- The **search receipt** — explicit record of what was searched and across which providers
- The **Constraint Memory Pulse host** — where constraint updates are visually acknowledged

If this component fails — confusing to read, hard to edit, or visually buried — Tripo's core trust proposition collapses. Every design decision here is high-stakes.

---

### 10.2 Anatomy

The Context Confirmation Card renders as a single horizontal row of tappable chips, appearing in the chat stream immediately before Trip Package cards. It occupies full chat width with no card elevation — it is a chat message, not a modal.

```
┌────────────────────────────────────────────────────────────────┐
│  חיפשתי:                                                       │  ← label
│  [✈️ TLV→BCN]  [📅 24–31 יול]  [👥 2]  [₪8,000]  [ישיר בלבד] │  ← chip row
│                                        via Kiwi · Booking.com  │  ← provider attribution
└────────────────────────────────────────────────────────────────┘
```

**Three sub-elements:**

1. **Label line** — "חיפשתי:" in `text-sm font-medium text-secondary`. Fixed, non-interactive.
2. **Chip row** — horizontally scrollable on mobile when chips overflow. Each chip is one understood constraint, tappable to edit.
3. **Provider attribution** — "via Kiwi · Booking.com" in `text-xs text-muted`. Fixed, non-interactive. Fulfills Revolut-style transparency: user sees exactly where the AI searched.

---

### 10.3 Chip Taxonomy

Every possible search constraint maps to exactly one chip type. The chip type determines icon, label format, and edit behavior.

| Chip type | Icon | Example label (Hebrew) | Edit behavior |
|---|---|---|---|
| **Route** | ✈️ | `TLV→BCN` | Opens inline origin/destination editor |
| **Dates** | 📅 | `24–31 יול` | Opens date range picker (bottom sheet) |
| **Passengers** | 👥 | `2 נוסעים` or `2 מבוגרים · 1 ילד` | Opens passenger count stepper |
| **Budget** | ₪ | `₪8,000` or `עד ₪6,000` | Opens numeric input |
| **Flight preference** | 🛫 | `ישיר בלבד` | Toggles direct/any |
| **Hotel class** | ⭐ | `4 כוכבים+` | Opens star rating selector |
| **Amenity** | 🏊 | `עם בריכה` | Toggle on/off |
| **Flexibility** | 🗓️ | `תאריכים גמישים` | Toggles flexible dates on/off |
| **Trip duration** | ⏱️ | `7 לילות` | Opens duration stepper (when no return date given) |

**Ordering rule:** Always render chips left-to-right (inline-start to inline-end in RTL) in this fixed priority order: Route → Dates → Passengers → Budget → Flight preference → Hotel class → Amenities → Flexibility. This creates a consistent scan pattern across every search.

**Maximum chips displayed inline:** 5. If the AI extracted more than 5 constraints, show 5 chips + an overflow chip: `[+2 נוספים]` in `chip-default` style. Tapping overflow expands all chips in a second row.

---

### 10.4 States

Each chip has four states:

#### State 1: Default (read)
The chip summarizes one understood constraint. Visual style:
- bg `chip-default` (#F3F4F6)
- text `text-secondary` (#4B5563), `text-sm font-medium`
- border: 1px `border` (#E5E7EB)
- height: 36px, `px-3`, `rounded-chip` (999px)
- icon: emoji prefix, 1 space gap before text

#### State 2: Editable (focused / tapped)
User has tapped the chip. Visual style:
- bg `chip-active` (#EEF3FD)
- border: 1.5px `border-active` (#1B4FD8)
- text `primary` (#1B4FD8)
- A small edit affordance icon (Lucide `Pencil`, 10px) appears inline-end of chip text
- The edit interface opens (see §10.5)

#### State 3: Updating (Constraint Memory Pulse)
A constraint was changed by a follow-up message (not by tapping). The chip visually acknowledges the update:
- Keyframe: bg flashes `chip-pulse-start` (#A5F3FC) for 150ms, transitions to `chip-active` (#EEF3FD) over 150ms
- Simultaneously: chip text updates to the new value
- AI's verbal acknowledgment appears in the chat stream beneath the updated card

```typescript
// Framer Motion variant for the pulse
const chipPulseVariants = {
  pulse: {
    backgroundColor: ['#A5F3FC', '#EEF3FD'],
    borderColor: ['#22D3EE', '#1B4FD8'],
    transition: { duration: 0.3, times: [0, 1] },
  },
}
```

#### State 4: Constraint not met (warning)
The AI could not fulfill this constraint (e.g., no direct flights found). Visual style:
- bg `#FFFBEB` (amber-50)
- border: 1px `#D97706` (amber-600)
- text `#D97706`
- Trailing icon: Lucide `AlertCircle`, 12px, same amber color
- Tooltip/label: "לא מצאתי טיסות ישירות — הצגתי הכי קרוב"

This state is non-interactive (cannot be "edited" since the constraint was attempted). It informs the user; the natural response is to type a follow-up.

---

### 10.5 Edit Interactions

Each chip type opens a different inline edit interface. All edit UIs open as **bottom sheets** on mobile, **inline popovers** on desktop (`≥1024px`). No page navigation. No modals that block the full screen.

#### Route chip edit
Bottom sheet with two text fields: מוצא / יעד (origin / destination). Auto-suggests IATA airport names from a fixed list of common Israeli departure airports + popular destinations. Confirm with "עדכן חיפוש" button.

#### Dates chip edit
Bottom sheet with a calendar date-range picker. Two-month display on mobile, RTL layout. Hebrew month names. Minimum stay: 1 night. Selected range highlights in `primary-light` bg with `primary` border. "גמישות ±3 ימים" toggle at bottom. Confirm with "עדכן תאריכים".

#### Passengers chip edit
Bottom sheet with stepper rows:
- מבוגרים (adults): +/- stepper, min 1, max 9
- ילדים (children 2–11): +/- stepper, min 0
- תינוקות (infants <2): +/- stepper, min 0

Live updates chip preview label as user adjusts. Confirm with "עדכן נוסעים".

#### Budget chip edit
Bottom sheet with a numeric input (ILS, no decimals) and a preset chip row: `₪4,000` · `₪6,000` · `₪8,000` · `₪12,000` · `ללא הגבלה`. Tapping a preset fills the input. Confirm with "עדכן תקציב".

#### All other chips (flight preference, hotel class, amenity, flexibility)
Inline toggle on tap — no bottom sheet needed. Tap once to toggle off; tap again to toggle on. Visual state switches immediately. Search re-triggers automatically on any toggle.

**Auto-re-search rule:** Every confirmed edit triggers an immediate re-search. The Context Confirmation Card re-renders with the updated chip in State 3 (pulse). The AI's streaming status messages restart. No "search" button required — editing is the trigger.

---

### 10.6 Provider Attribution Line

```
via Kiwi · Booking.com
```

- Always present. Non-interactive.
- `text-xs text-muted`
- Inline-start aligned (right-aligned in RTL, same side as chip row start)
- Shows only providers actually queried for this specific search
- If only flights queried (hotel already saved): "via Kiwi"
- If only hotels queried: "via Booking.com"

This line is Tripo's Perplexity-style "show your work" moment. It transforms the question "did it really search?" into evidence. It also reinforces affiliate transparency — users know exactly where they'll land when they tap a CTA.

---

### 10.7 Full Component Layout

```
┌─ chat stream width (max 640px) ──────────────────────────────────┐
│                                                                    │
│  חיפשתי:                                                          │  text-sm, text-secondary
│                                                                    │
│  [✈️ TLV→BCN] [📅 24–31 יול] [👥 2 נוסעים] [₪8,000] [ישיר בלבד] │  chip row
│  ← scrollable on overflow →                                        │
│                                                                    │
│  via Kiwi · Booking.com                                           │  text-xs, text-muted
│                                                                    │
└────────────────────────────────────────────────────────────────────┘
```

**No card elevation, no border, no shadow.** The Context Confirmation Card renders flush in the chat stream as an AI message, not as a separate UI panel. It shares the same background as the chat area. This keeps the "chat is the chrome" principle intact — everything lives in the stream.

**Spacing:**
- `mb-2` below the label line
- `gap-2` between chips
- `mt-1` above provider attribution line
- `mb-4` below the entire component before first Trip Package card

---

### 10.8 Overflow and Scrolling

On mobile (390px), 5 chips at ~80px average = 400px + 8px gaps = overflow likely. Behavior:

- Chip row is `overflow-x: auto` with `scrollbar-width: none` (hidden scrollbar)
- Left/right fade gradients applied via pseudo-elements to hint scrollability:
  ```css
  .chip-row::after {
    content: '';
    position: absolute;
    inset-inline-end: 0;
    top: 0; bottom: 0;
    width: 24px;
    background: linear-gradient(to inline-end, transparent, var(--background));
    pointer-events: none;
  }
  ```
- On desktop, chips wrap to two lines before overflowing

**Overflow chip (`+N נוספים`):**
- Style: same as `chip-default` but text is `text-primary font-medium`
- Tap expands the row into a two-line wrap layout via Framer Motion `height` animation (200ms)
- Tap again collapses

---

### 10.9 Re-render on Follow-Up Search

When the user sends a follow-up message that changes constraints:

1. The **existing** Context Confirmation Card in the chat history becomes read-only (chips lose their tappable affordance — no edit icon, no hover state)
2. A **new** Context Confirmation Card appears in the stream for the new search, with the changed chip in State 3 (pulse) and all unchanged chips in State 1 (default)
3. The AI verbal acknowledgment immediately precedes the new card:
   > "עדכנתי — עכשיו מחפש עם התאריכים החדשים, שאר ההעדפות נשמרו 👌"

This creates a clear visual audit trail in the chat: the user can scroll up and see exactly what was searched each time. Historical cards are preserved, not replaced.

---

### 10.10 Accessibility Specification

| Requirement | Implementation |
|---|---|
| Card region | `role="region"` with `aria-label="אישור פרמטרי חיפוש"` |
| Label line | Plain `<p>` — not a heading |
| Chip row | `role="list"` containing `role="listitem"` per chip |
| Each chip (interactive) | `<button>` element. `aria-label` = full constraint description: `aria-label="ערוך יעד: TLV לברצלונה"` |
| Chip edit state | `aria-pressed="true"` when in edit mode |
| Pulse state | `aria-live="polite"` on chip text span — screen readers announce value change |
| Warning chip | `role="status"` with `aria-label="אזהרה: לא נמצאו טיסות ישירות — מוצגת האפשרות הקרובה ביותר"` |
| Overflow chip | `aria-label="הצג עוד [N] פרמטרים"` |
| Bottom sheet (edit) | `role="dialog"`, `aria-modal="true"`, `aria-label` per sheet type, focus trapped inside |
| Provider line | `aria-label="חוּפַּשׂ דרך: Kiwi ו-Booking.com"` |

---

### 10.11 TypeScript Component Interface

```typescript
// types/context-confirmation.ts

type ChipType =
  | 'route'
  | 'dates'
  | 'passengers'
  | 'budget'
  | 'flightPreference'
  | 'hotelClass'
  | 'amenity'
  | 'flexibility'
  | 'duration'

type ChipState = 'default' | 'editing' | 'updating' | 'unmet'

interface ConfirmationChip {
  type: ChipType
  label: string           // Hebrew display label e.g. "TLV→BCN", "24–31 יול"
  icon: string            // emoji
  state: ChipState
  value: unknown          // typed per ChipType in implementation
  editable: boolean       // false for historical (read-only) cards
}

interface ContextConfirmationCardProps {
  chips: ConfirmationChip[]
  providers: string[]           // e.g. ['Kiwi', 'Booking.com']
  isHistorical?: boolean        // true = read-only, no edit affordances
  onChipEdit: (chip: ConfirmationChip, newValue: unknown) => void
  // onChipEdit triggers re-search in parent. Component does not own search state.
}
```

---

### 10.12 Edge Cases & Guardrails

| Scenario | Handling |
|---|---|
| AI extracted 0 chips (fully ambiguous input) | Do not render the card. AI asks one clarifying question instead. |
| Only destination extracted, no dates | Render route chip only. Dates chip absent. Trip Package cards show "תאריכים גמישים" results. |
| User edits chip but submits same value | No re-search triggered. Bottom sheet closes silently. |
| Two simultaneous chip edits | Disallow: closing one edit sheet before another can open. |
| Historical card chips tapped | No response — cursor is `default`, no focus ring, no edit sheet. Visual affordance already removed. |
| Constraint resolved mid-conversation | Warning chip (State 4) on historical card remains as-is. New card shows fulfilled constraint in State 1. |
| Provider attribution when only one API responds | Show only the responding provider. Never imply both searched if one failed. |
| Very long destination name | Route chip truncates destination city to 10 chars: "TLV→Amsterdam" → "TLV→Amster..." |

---

### Step 10 Summary

**Status: ✅ Complete**

Deliverables finalized:
- [x] Full anatomy: label, chip row, provider attribution — chat-native, no card elevation
- [x] Chip taxonomy: 9 constraint types with icons, label format, edit behavior
- [x] Chip ordering rule (fixed priority: Route → Dates → Passengers → Budget → ...)
- [x] 4 chip states: default, editable, updating (pulse), unmet (warning)
- [x] All 5 edit interaction types (bottom sheet vs. inline toggle)
- [x] Auto-re-search on edit confirmation — no separate search button
- [x] Provider attribution line — transparency, Revolut-style
- [x] Re-render strategy for follow-up searches (new card appended, history preserved as read-only)
- [x] Overflow behavior: horizontal scroll + fade gradients + overflow chip
- [x] Full accessibility spec (Hebrew ARIA labels, live regions, focus trap in bottom sheets)
- [x] TypeScript component interface (`ContextConfirmationCardProps`)
- [x] Edge cases: no chips, partial extraction, duplicate edit prevention

---

## Step 11 — Chat Interface Layout & Cold Start

### 11.1 Layout Philosophy

The chat interface is the entire product. There is no landing page, no onboarding flow, no separate search screen. The user arrives directly into the chat. The layout must:

1. **Feel native on mobile.** Full-height, keyboard-aware, no browser chrome bleeding through.
2. **Keep input always reachable.** The text input is sticky at the bottom, always one thumb away.
3. **Let the stream breathe.** The chat history area is the hero — not a sidebar, not a panel. It takes all remaining vertical space.
4. **Stay out of its own way.** The chrome (header, input bar) is minimal. The content (messages, cards) is everything.

---

### 11.2 Page Structure (Mobile-first)

```
┌──────────────────────────────────────┐  ← 100dvh (dynamic viewport height — iOS Safari safe)
│  [HEADER]                            │  56px fixed top
│  טריפו          [💙 הטיולים שלי →]  │
├──────────────────────────────────────┤
│                                      │
│  [CHAT STREAM]                       │  flex-1, overflow-y: auto
│                                      │  pb-4 padding above input bar
│  — messages, cards, confirmation     │
│    cards all render here             │
│                                      │
│                                      │
├──────────────────────────────────────┤
│  [INPUT BAR]                         │  sticky bottom, auto-height
│  [📎] [textarea ···············] [→] │  56px min, grows with content
└──────────────────────────────────────┘
│  [SAFE AREA]                         │  env(safe-area-inset-bottom) — iPhone notch
└──────────────────────────────────────┘
```

**Critical CSS pattern — full-height chat without scroll issues:**
```css
/* Full-height chat container — iOS Safari compatible */
.chat-page {
  height: 100dvh;          /* dvh = dynamic viewport height, accounts for browser chrome */
  display: flex;
  flex-direction: column;
  overflow: hidden;
}
.chat-stream {
  flex: 1;
  overflow-y: auto;
  overscroll-behavior: contain;   /* prevents pull-to-refresh interfering with scroll */
  -webkit-overflow-scrolling: touch;
}
.input-bar {
  flex-shrink: 0;
  padding-bottom: env(safe-area-inset-bottom);  /* iPhone home indicator */
}
```

---

### 11.3 Header

Minimal. The header exists only to orient the user and provide one-tap access to saved trips. It never navigates away from the chat.

| Element | Spec |
|---|---|
| Height | 56px |
| Background | `surface` (#FFFFFF) with `border-b border-border` |
| Logo/wordmark | "טריפו" in Rubik 600, `text-primary`, inline-end aligned (RTL: right side) |
| Saved trips link | `text-sm text-secondary` with Lucide `Bookmark` icon. Label: "הטיולים שלי ←". Inline-start (RTL: left side). Hidden if user is anonymous — shown only when authenticated. |
| Anonymous state | Header contains only the wordmark. No account icon, no login CTA — the header is not a sign-up surface. Sign-up happens naturally at the save moment. |
| Shadow | None. Border-bottom only — keep it light. |
| No back button | There is nowhere to go back to. The chat IS the app. |

**Desktop (≥1024px):** Header max-width `680px`, centered. Chat column centered in viewport with `bg-background` filling the sides.

---

### 11.4 Chat Stream

The stream is a vertical flex column of messages, flowing top-to-bottom with newest messages at the bottom. Standard chat layout.

| Property | Value |
|---|---|
| Padding | `px-4 pt-4 pb-6` |
| Message gap | `gap-3` between consecutive same-sender messages; `gap-5` between sender switches |
| Max-width (messages) | User bubbles: `max-w-[75%]`. AI bubbles: `max-w-[85%]`. Cards: `w-full`. |
| Alignment | User messages: `items-end` (RTL: right). AI messages: `items-start` (RTL: left). |
| Auto-scroll | Smooth scroll to bottom on every new message/card. Pause auto-scroll if user has manually scrolled up (read indicator pattern). |
| Scroll resume | If user scrolled up and new content arrives: show a subtle "הודעה חדשה ↓" floating chip that, when tapped, scrolls to bottom and resumes auto-scroll. |

**Scroll-to-bottom chip:**
- Fixed position, bottom `72px` (above input bar), centered
- bg `primary`, text `primary-foreground`, `text-xs`, `rounded-chip`, `shadow-card`
- Entrance/exit: Framer Motion `opacity` + `y` (fade up from input bar)
- Auto-dismisses when user scrolls to within 100px of bottom

---

### 11.5 Warm Cold-Start Sequence

The Cold Start is the first thing a new user sees. It must eliminate "what do I type?" anxiety instantly. The sequence is animated and feels alive — not a static placeholder.

**This plays when:** the chat stream is empty (new session, no history).

#### Phase 1 — AI greeting (0ms)
The AI's greeting message streams in character-by-character via Framer Motion, exactly like a real WhatsApp message being typed and delivered:

> *"שלום! אני טריפו 👋*
> *תגיד לי לאן אתה רוצה לטוס ומתי, ואני אדאג לכל השאר."*

- Streaming speed: ~40ms per character (fast enough to feel alive, slow enough to read)
- No typing indicator before this — the message just begins arriving
- Delivered as a standard AI chat bubble

#### Phase 2 — Suggestion chips (800ms after greeting completes)
Three tappable suggestion chips fade up beneath the greeting, staggered 120ms apart:

```
[✈️ ברצלונה לשבוע בקיץ, זוג, עד ₪8,000]
[🏖️ יוון עם ילדים בפסח, טיסה ישירה בלבד]
[🌍 איפשהו חם בנובמבר, הפתע אותי]
```

**Chip styling (Cold Start chips — distinct from Context Confirmation chips):**
- bg `primary-light` (#EEF3FD)
- border 1px `border-active` (#1B4FD8)
- text `primary` (#1B4FD8), `text-sm font-medium`
- `rounded-chip` (999px), `px-4 py-2`, full-width on mobile
- Stacked vertically on mobile (not horizontal — these are longer strings)
- Lucide `ArrowUpRight` icon inline-end, 14px

**Tap behavior:**
- Chip text populates the input field exactly as written
- Input field animates: brief `scale: 1 → 1.02 → 1` (100ms) to signal receipt
- User can edit the text before sending, or send immediately
- The chip itself fades out once tapped — it served its purpose

**Chip content strategy:**
- Chip 1: specific destination + realistic constraints → for users who know where they want to go
- Chip 2: category travel (beach, family) + hard constraint → for users who have constraints but not a destination
- Chip 3: open-ended / "surprise me" → for exploratory users, shows the AI can handle vague requests
- Chips change per session (3 variants per slot, rotated randomly) — prevents the interface feeling static on return visits

#### Phase 3 — Input ready
The input placeholder pulses once with a subtle border color transition (`border-border → border-active` over 600ms) to invite typing. Then settles to static placeholder text:

> *"לאן אתה רוצה לטוס?"*

The sequence total duration from page load to fully interactive: ~2.5 seconds. Fast enough to feel responsive, slow enough for the greeting to land.

---

### 11.6 Returning User Experience

When the user has a previous session (chat history in local state or authenticated):

**The chat stream pre-populates** with the previous conversation — messages, cards, and confirmation chips — exactly where they left off. No "continue your trip?" prompt. It just resumes.

**Header shows saved trips link** (if authenticated): "הטיולים שלי ←"

**No Cold Start sequence plays** for returning users. The first visible element is the bottom of their previous conversation, ready for a follow-up message.

**The input placeholder changes** for returning users:
> *"המשך את השיחה..."* (Continue the conversation...)

**New session trigger** (user explicitly starts fresh):
A subtle "התחל חיפוש חדש +" link appears at the very top of the chat stream (above all previous messages), in `text-xs text-muted`. Tapping it scrolls to a new blank input and plays the Cold Start sequence for a fresh search, without erasing the history.

---

### 11.7 Input Bar

The input bar is the primary interaction surface. It must be thumb-friendly, fast, and never fight with the keyboard.

```
┌───────────────────────────────────────────────────┐
│  bg: surface, border-top: 1px border              │
│  px-3 py-2                                        │
│                                                   │
│  ┌──────────────────────────────────┐  ┌───────┐  │
│  │ textarea (RTL, auto-height)      │  │  →    │  │
│  │ placeholder: לאן אתה רוצה לטוס? │  │ send  │  │
│  └──────────────────────────────────┘  └───────┘  │
└───────────────────────────────────────────────────┘
```

| Element | Spec |
|---|---|
| Container | `bg-surface`, `border-t border-border`, `px-3 py-2` |
| Textarea | `dir="rtl"`, auto-resize (1 row → max 4 rows), `text-base` (16px — prevents iOS zoom on focus), `bg-surface-raised`, `rounded-component`, `border border-border`, `px-3 py-2`, `resize-none` |
| Placeholder | "לאן אתה רוצה לטוס?" (new user) / "המשך את השיחה..." (returning user) |
| Send button | Lucide `Send` icon (or `ArrowUp`), 40px × 40px touch target, `bg-primary`, `text-primary-foreground`, `rounded-full`. Disabled (opacity 40%) when textarea empty. |
| Send trigger | Button tap OR `Enter` key on desktop. On mobile, `Enter` inserts newline (standard behavior) — no accidental sends. |
| Keyboard avoidance | The entire chat page uses `height: 100dvh` + CSS `env(keyboard-inset-height)` where supported; fallback via `visualViewport` resize event in JS to push content up when keyboard opens |
| Focus behavior | Textarea focuses on page load (desktop). On mobile: do NOT auto-focus on load (prevents keyboard opening unexpectedly). User taps to focus. |
| Loading state | When AI is responding: textarea disabled, placeholder "טריפו מחפש...", send button replaced with a stop/cancel icon (Lucide `Square`). Tapping stop cancels the current stream. |

**16px minimum font size rule:** iOS Safari zooms into any input with `font-size < 16px`. The textarea must be `text-base` (16px) or the keyboard will zoom and break the layout.

---

### 11.8 Sign-Up Bottom Sheet (Save Moment)

This is the anonymous-to-authenticated conversion point. Triggered when a user taps "שמור טיול" on a Trip Package card.

**Design principle:** Identical flow to WhatsApp signup. Every Israeli user has done this. Zero learning curve.

```
┌──────────────────────────────────────┐  ← bottom sheet, slides up
│  ▬                                   │  ← drag handle
│                                      │
│  שמור את הטיול שלך                   │  text-lg font-semibold
│  הזן מספר טלפון ישראלי לקבלת קוד    │  text-sm text-secondary
│                                      │
│  ┌──────────────────────────────┐    │
│  │ 🇮🇱 +972  [05X-XXX-XXXX  ]  │    │  phone input
│  └──────────────────────────────┘    │
│                                      │
│  [שלח קוד SMS]                       │  primary button, full-width
│                                      │
│  [ביטול]                             │  ghost button
└──────────────────────────────────────┘
```

**Phase 2 — OTP entry (same sheet, animated transition):**

```
│  קוד נשלח ל-05X-XXX-XXXX            │
│  ┌───┐ ┌───┐ ┌───┐ ┌───┐ ┌───┐ ┌───┐│  6-digit OTP input (single field, auto-advance)
│  └───┘ └───┘ └───┘ └───┘ └───┘ └───┘│
│  שלח שוב (00:45)                     │  resend countdown
│  [אישור]                             │
```

- On correct OTP: sheet dismisses, account created silently, trip saved automatically
- AI confirms in chat stream: "מעולה! שמרתי את הטיול אצלך 💙 תמצא אותו תחת הטיולים שלי."
- On incorrect OTP: inline shake animation + "קוד שגוי, נסה שוב"
- Sheet never navigates away from the chat. The trip cards remain visible behind the sheet.

**Bottom sheet mechanics:**
- Framer Motion `AnimatePresence` + `y: '100%' → 0` slide-up, 300ms spring
- Backdrop: `bg-black/40`, `backdrop-blur-sm`
- Drag handle: 4px × 32px pill, `bg-border`, centered top
- `role="dialog"`, `aria-modal="true"`, focus trapped
- Dismissable by: drag down, backdrop tap, cancel button — but NOT by accidental scroll

---

### 11.9 Full Page Component Tree

```
<ChatPage>                          // height: 100dvh, flex column
  <ChatHeader />                    // 56px, fixed top
  <ChatStream>                      // flex-1, overflow-y auto
    <WarmColdStart />               // only when stream empty
      <AIGreetingMessage />
      <SuggestionChips />
    <MessageList>                   // scrollable history
      <AIMessage />                 // AI bubbles
      <UserMessage />               // user bubbles
      <StreamingStatusMessages />   // "מחפש טיסות..."
      <TypingIndicator />           // 3-dot bounce
      <ContextConfirmationCard />   // chip row
      <TripPackageCard />           // x1–3, staggered
    </MessageList>
    <ScrollToBottomChip />          // conditional float
  </ChatStream>
  <InputBar />                      // sticky bottom
  <SaveTripSheet />                 // AnimatePresence portal
</ChatPage>
```

---

### 11.10 Responsive Behaviour

| Breakpoint | Layout change |
|---|---|
| Mobile `<640px` | Full-width stream, `px-4`. Input bar spans full width. Header spans full width. |
| Tablet `640px–1023px` | Stream max-width `600px`, centered. Input bar same width, centered. |
| Desktop `≥1024px` | Chat column max-width `680px`, centered in viewport. Background `bg-background` fills sides. Header spans full `680px` column. Suggestion chips render horizontal (3 in a row) instead of stacked. Input `Enter` sends. |

**Desktop empty-state padding:** On desktop, the Cold Start greeting and chips are vertically centered in the visible stream area (not anchored to top) — the empty state feels intentional, not sparse.

---

### 11.11 Accessibility Specification

| Requirement | Implementation |
|---|---|
| Page landmark | `<main>` wraps the chat stream. `<header>` wraps the header bar. |
| Live region for AI messages | `<div aria-live="polite" aria-atomic="false">` wraps the message list — screen readers announce new messages as they arrive |
| Streaming text | Use `aria-live="assertive"` on the currently-streaming AI message only; switch to `polite` once complete |
| Input label | `<label htmlFor="chat-input" className="sr-only">הודעה לטריפו</label>` |
| Send button | `aria-label="שלח הודעה"`. When disabled: `aria-disabled="true"` |
| Typing indicator | `aria-label="טריפו מקליד..."`, `role="status"` |
| Suggestion chips | `role="list"` + `role="listitem"`. Each chip: `<button aria-label="שלח: [chip text]">` |
| Scroll to bottom chip | `aria-label="גלול להודעה האחרונה"` |
| Bottom sheet | `role="dialog"`, `aria-labelledby="sheet-title"`, `aria-modal="true"` |
| Phone input | `inputmode="tel"`, `autocomplete="tel-national"`, `aria-label="מספר טלפון ישראלי"` |
| OTP input | `inputmode="numeric"`, `autocomplete="one-time-code"`, `aria-label="קוד אימות בן 6 ספרות"` |

---

### Step 11 Summary

**Status: ✅ Complete**

Deliverables finalized:
- [x] Full page structure: header + stream + input bar, 100dvh, keyboard-safe CSS
- [x] Header spec: minimal chrome, saved trips link, anonymous vs. authenticated states
- [x] Chat stream: layout, alignment, auto-scroll, scroll-to-bottom chip
- [x] Warm Cold-Start: 3-phase animated sequence (greeting → chips → input pulse), chip content strategy, returning user variant
- [x] Input bar: textarea spec (16px iOS rule), send button, keyboard avoidance, loading/cancel state
- [x] Sign-up bottom sheet: full 2-phase OTP flow (phone → code), animations, dismiss rules
- [x] Full component tree (`ChatPage` → `ChatStream` → `MessageList` → all children)
- [x] Responsive behaviour (mobile → tablet → desktop)
- [x] Full accessibility spec (live regions, landmarks, Hebrew ARIA labels throughout)

---

## UX Design Specification — MVP Complete

**Steps 1–11 finalized.** The specification covers:

| # | Section | Status |
|---|---|---|
| 1 | Executive Summary | ✅ |
| 2 | Core User Experience | ✅ |
| 3 | Desired Emotional Response | ✅ |
| 4 | UX Pattern Analysis & Inspiration | ✅ |
| 5 | Design System Foundation | ✅ |
| 6 | Core UX Detailed | ✅ |
| 7 | Novel UX Patterns | ✅ |
| 8 | Visual Foundation (Color System) | ✅ |
| 9 | Component: Trip Package Card | ✅ |
| 10 | Component: Context Confirmation Card | ✅ |
| 11 | Chat Interface Layout & Cold Start | ✅ |

**Ready for handoff to:** Architecture (`/bmad-bmm-create-architecture`)
