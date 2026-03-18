---
stepsCompleted: ['step-01-init', 'step-02-discovery', 'step-02b-vision', 'step-02c-executive-summary', 'step-03-success', 'step-04-journeys', 'step-05-domain', 'step-06-innovation', 'step-07-project-type', 'step-08-scoping', 'step-09-functional', 'step-10-nonfunctional', 'step-11-polish']
inputDocuments: ["c:\\Users\\User\\OneDrive\\Desktop\\TRIPO\\_bmad-output\\planning-artifacts\\product-brief-TRIPO-2026-03-12.md"]
briefCount: 1
researchCount: 0
brainstormingCount: 0
projectDocsCount: 0
workflowType: 'prd'
classification:
  projectType: web_app
  domain: general
  complexity: medium
  projectContext: greenfield
---

# Product Requirements Document - TRIPO

**Author:** Liad
**Date:** 2026-03-12

## Executive Summary

Tripo is a conversational web application designed to eliminate the fragmented nature of travel booking by acting as a personal AI agent that handles all searching, context-gathering, and compiling in one place. It targets Israeli solo travelers, couples, and families who experience significant friction and decision fatigue when coordinating trip planning. By aggregating live data across multiple providers (Kiwi, Booking.com) and presenting unified, context-aware suggestions for common Israeli routes, Tripo replaces the multi-tab browser experience with a single, conversational checkout flow that users can complete in under 10 minutes. The entire UI serves Israeli audiences with native Hebrew support and RTL layout requirements.

**The Differentiator:** 
Tripo differentiates itself through persistent contextual memory. Unlike existing solutions that only solve for search, Tripo treats the entire trip planning experience as one continuous, context-aware conversation in Hebrew. It remembers nuanced user constraints (such as "traveling with a toddler" or "window seat required") collected throughout the dialogue and automatically applies them to all subsequent searches, pricing options in ILS (₪), without requiring repetitive form-filling.

## Success Criteria

### User Success
- **Actionable Outcomes:** Session completes with at least 1 affiliate link clicked.
- **Speed to Value:** Time from the first conversational message to an affiliate link click is under 10 minutes.
- **Retention:** User returns to plan a second trip within 90 days.

### Business Success
- **3 Months:** Acquire 500 registered users, achieve 100+ affiliate link clicks per month, deploy 2 live API integrations.
- **12 Months:** Scale to 10,000 registered users, generate ₪20,000/month in affiliate revenue, expand to 4+ live integrations, maintain 20% MoM growth.

### Technical Success
- **Localization:** 100% RTL layout compliance and Hebrew language support across the entire app.
- **Context Persistence:** AI successfully retains constraints across 100% of user turns in a single session.
- **Performance:** Chat response time and API aggregation latency is under 5 seconds per complex trip compilation.

## Product Scope

### MVP Strategy & Philosophy
**MVP Approach:** Experience MVP focused on demonstrating the core "wow" factor of conversational, context-aware travel planning for the Israeli market. 
**Development:** Solo founder + AI. Target launch in 8-10 weeks. 

### MVP Feature Set (Phase 1)
- User Accounts: Registration, login, save trips (Israeli phone number auth). Open anonymous browsing until save.
- Conversational Interface: AI chat experience (Hebrew only, RTL interface).
- Contextual Memory: Persistent constraint tracking within a session.
- Core Integrations: Strictly scoped to One-way flights (TLV origin only) via Kiwi, and Hotel search via Booking.com. 
- Curated Delivery: Aggregated cards with affiliate links, displaying prices in ILS (₪) alongside source currency, max 3 destinations suggested.

### Post-MVP Growth
- **Phase 2:** Round-trip and multi-city flights, additional departure airports, car rentals, localized activities, price drop alerts.
- **Phase 3:** Proactive AI trip suggestions, social group collaboration, iOS/Android mobile apps, B2B white-labeling.

## User Journeys

### 1. The Overwhelmed Planner (Primary Happy Path)
**Situation:** Sarah (32, Marketing Manager from TLV) is exhausted and dreads spending hours opening browser tabs to plan a getaway to Lisbon. She has a ₪6,000 budget and wants a boutique hotel.
**Action:** She opens Tripo (fully RTL Hebrew UI) and types naturally: "I need to fly to Lisbon with my partner the last week of July. We have ₪6,000 and want a boutique hotel in the center."
**Resolution:** In under 15 seconds, Tripo queries Kiwi (from TLV) and Booking.com, replying conversationally with three curated "Trip Packages." She clicks "Book Flights" and "Book Hotel" directly from the chat card. The trip is automatically saved for her.

### 2. The Family Trip Coordinator (Complex Constraints)
**Situation:** David (41, Father of two from Ramat Gan) needs a family vacation but is anxious about connecting flights with a toddler. 
**Action:** David tells Tripo his budget and family size. When Tripo suggests Dubai, he adds constraints: "I absolutely need direct flights, we can't do layovers with the toddler. Make sure the hotel has a pool." 
**Resolution:** Tripo filters all searches out of TLV. When David subsequently asks to "push this back by one week," Tripo remembers all constraints (toddler, direct flight, pool, budget) and seamlessly updates the cards for the new dates.

### 3. Platform Admin (Operations)
**Situation:** Alex (Operations Lead) logs into the Hebrew Admin Dashboard to check API health.
**Action/Resolution:** Alex views the "Integration Health" dashboard, noting a latency spike from Booking.com but verifying CTR remains high at 45%. Alex logs a bug report, confident the platform is monetizing sessions correctly.

## Domain-Specific Requirements

### Compliance & Regulatory
- **Israeli Consumer Protection Law:** All results must include a clear Hebrew disclaimer indicating Tripo is an aggregator: "טריפו הינה פלטפורמת השוואה. ההזמנה מתבצעת ישירות מול הספק" (Tripo is a comparison platform. Booking is made directly with the provider).
- **Privacy & Data Retention:** Conversation logs stored for a maximum of 30 days before auto-deletion. Explicit consent checkbox required during user signup for AI memory storage. No sensitive PII (passports) may ever be stored.

## Innovation & Novel Patterns

- **Context-as-a-Service:** Challenging the reliance on form-based search by proving that continuous, context-aware conversations outperform traditional UX.
- **Context Confirmation Card:** A novel UX element explicitly summarizing the AI's understanding in Hebrew at the top of results. Users tap to correct constraints instantly, replacing traditional complex filtering trees.
- **Zero-Liability Value:** The affiliate-only model provides full travel orchestration value with zero booking liability.

## Technical Architecure (Web App)

- **Application Framework:** Next.js (React) for robust component-based architecture, SPA routing, and native i18n configurations for strict RTL Hebrew layouts.
- **Browser Support:** Evergreen modern browsers only (Chrome, Safari, Edge, Firefox). Minimal SSR reserved for the marketing landing page for SEO; chat interface is CSR only.
- **Real-Time Data Streams:** Implementation of WebSockets for the chat AI interface to provide a streaming response effect. REST polling utilized for the aggregator API payloads.

## Functional Requirements (FRs)

### Conversational Interface
- FR1: Users can interact with the system using natural language input in Hebrew.
- FR2: The system can parse user constraints (dates, budget in ILS, destinations, amenities, passenger count) from unstructured Hebrew text.
- FR3: The system can present curated trip results as interactive UI cards within the chat stream.
- FR4: Users can click affiliate action buttons directly from the interactive cards to be redirected to the provider.
- FR5: The system can display a "Context Confirmation Card" summarizing understood constraints in Hebrew at the top of the results.
- FR6: Users can tap individual elements within the "Context Confirmation Card" to edit or correct the constraints directly.
- FR7: In the event of zero aggregator results, the system can output a fallback conversational message in Hebrew explaining the limitation.

### Search & Aggregation (MVP Scoped)
- FR8: The system can simultaneously query the Kiwi API for one-way flights originating strictly from Tel Aviv (TLV).
- FR9: The system can simultaneously query the Booking.com API for hotel availability matching the specified dates and locations.
- FR10: The system can filter and aggregate raw API responses into a maximum of 3 cohesive "Trip Packages" per user query.
- FR11: The system can dynamically convert and display foreign currency pricing from the APIs into Israeli New Shekels (ILS).
- FR12: The system can fetch and update the exchange rate for ILS every 24 hours.
- FR13: The system can cache frequent route/destination search results for up to 15 minutes.
- FR14: The system can queue incoming API requests to ensure a maximum concurrency limit of 3 simultaneous calls per provider.

### Context & Memory Management
- FR15: The system can retain all user-stated constraints (preferences, conditions, filters) across multiple turns of dialogue within a single active session.
- FR16: The system can automatically apply historical constraints from the current session to new searches without requiring the user to restate them.
- FR17: The system can overwrite a specific constraint (e.g., changing the budget) while retaining all other previously stated constraints.
- FR18: The system automatically purges all conversational memory logs 30 days after session creation.

### User Account Management
- FR19: Users can initiate and complete an entire trip search session anonymously without an account.
- FR20: The system can prompt an anonymous user to create an account only when they explicitly choose to "Save" a trip.
- FR21: Users can register and authenticate using an Israeli phone number (+972).
- FR22: Users can view a "Saved Trips Dashboard" containing all their previously saved trip packages and constraints.
- FR23: Users can re-initiate a chat session using a saved trip as the baseline context.

### Operations
- FR24: Admin users can access a secure internal dashboard localized in Hebrew.
- FR25: Admin users can view latency metrics for Kiwi and Booking.com API integrations.
- FR26: Admin users can view the aggregate Click-Through Rate (CTR) for generated affiliate links.
- FR27: The system can append affiliate tracking IDs to all outbound Booking.com and Kiwi URLs.

## Non-Functional Requirements (NFRs)

### Performance & Scalability
- **Latency:** The system responds with curated flight and hotel options within 5.0 seconds of the user's conversational prompt.
- **UI Responsiveness:** The chat interface renders at 60FPS during WebSocket AI streaming to prevent UI stuttering.
- **Boot Time:** The initial SPA Time to Interactive (TTI) is under 2.5 seconds over a 4G mobile network.
- **Concurrency:** The MVP architecture supports up to 100 concurrent chat sessions without exceeding the 5-second latency target.

### Accessibility & Localization
- **UI Directionality:** The entire application interface rigidly enforces a Right-to-Left (RTL) layout without breaking CSS containers.
- **Keyboard Navigability:** All interactive elements (affiliate action cards, Context Confirmation Card items) adhere to WCAG 2.1 AA keyboard navigability standards.
- **Screen Reader Support:** All dynamic prices, dates, and action buttons feature explicit Hebrew ARIA labels formatting data logically for Israeli screen readers.
- **Color Contrast:** Text-to-background contrast ratios meet or exceed WCAG 2.1 AA standards (minimum 4.5:1 for normal text).

### Integration & Reliability
- **API Fallback:** If aggregator APIs exceed a 4.5-second timeout threshold, the system gracefully fails the search for that specific vertical and informs the user in Hebrew, preventing a chat session crash.
- **Currency Accuracy:** Displayed ILS (₪) prices maintain an accuracy within a ±2% margin of error compared to the source API's native currency.
