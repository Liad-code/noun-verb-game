## Non-Functional Requirements

### Performance
- **Latency Target:** The system must respond with a complete set of curated flight and hotel options within 5 seconds of the user's conversational prompt.
- **UI Responsiveness:** The chat interface must maintain 60FPS rendering during the WebSocket streaming of the AI's response to prevent perceived UI stuttering.
- **Boot Time:** The initial SPA load time (Time to Interactive) must be under 2.5 seconds over a standard 4G mobile network.

### Scalability (MVP Scoped)
- **API Concurrency Limit:** The system must restrict simultaneous API querying to a maximum of 3 concurrent calls per provider (Kiwi, Booking.com) to prevent IP rate-limiting.
- **Concurrent Users:** The MVP architecture must support up to 100 concurrent chat sessions without exceeding the 5-second latency target.
- **Caching Degradation:** In the event of high traffic, the system should aggressively fall back to the 15-minute cached results for common routes (e.g., TLV to ATH) rather than hitting the live APIs.

### Accessibility & Localization
- **UI Directionality:** The entire application interface must rigidly enforce a Right-to-Left (RTL) layout without breaking CSS containers.
- **Keyboard Navigability (WCAG 2.1 AA):** All interactive elements, specifically the affiliate action cards and the "Context Confirmation Card" items, must be completely navigable via keyboard.
- **Screen Reader Support:** All dynamic prices (ILS and source currency), dates, and action buttons must feature explicit Hebrew ARIA labels formatting data logically for Israeli screen readers.
- **Color Contrast:** Text-to-background contrast ratios must meet or exceed WCAG 2.1 AA standards (minimum 4.5:1 for normal text).

### Integration & Reliability
- **API Fallback:** In the event the Booking.com or Kiwi APIs experience downtime or exceed a 4.5-second timeout threshold, the system must gracefully fail the search for that specific vertical and inform the user in Hebrew, rather than crashing the entire chat session.
- **Currency Exchange Refresh:** The system must fetch and update the exchange rate for ILS (₪) every 24 hours exactly to ensure price accuracy within the ±2% margin of error requirement.
