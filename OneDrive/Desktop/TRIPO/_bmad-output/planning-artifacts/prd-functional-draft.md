## Functional Requirements

### Conversational Interface
- FR1: Users can interact with the system using natural language input in Hebrew.
- FR2: The system can parse user constraints (dates, budget in ILS, destinations, amenities, passenger count) from unstructured Hebrew text.
- FR3: The system can present curated trip results as interactive UI cards within the chat stream.
- FR4: Users can click affiliate action buttons (e.g., "Book Flight", "Book Hotel") directly from the interactive cards to be redirected to the provider.
- FR5: The system can display a "Context Confirmation Card" summarizing understood constraints in Hebrew at the top of the results.
- FR6: Users can tap individual elements within the "Context Confirmation Card" to edit or correct the constraints directly.
- FR7: In the event of zero aggregator results, the system can output a fallback conversational message in Hebrew explaining the limitation.

### Search & Aggregation (MVP Scaled)
- FR8: The system can simultaneously query the Kiwi API for one-way flights originating strictly from Tel Aviv (TLV).
- FR9: The system can simultaneously query the Booking.com API for hotel availability matching the specified dates and locations.
- FR10: The system can filter and aggregate raw API responses into a maximum of 3 cohesive "Trip Packages" per user query.
- FR11: The system can dynamically convert and display all foreign currency pricing from the APIs into Israeli New Shekels (ILS) using a daily refreshed exchange rate.
- FR12: The system can cache frequent route/destination search results for up to 15 minutes to optimize API rate limits.
- FR13: The system can queue incoming API requests to ensure a maximum concurrency limit of 3 simultaneous calls per provider.

### Context & Memory Management
- FR14: The system can retain all user-stated constraints (preferences, conditions, filters) across multiple turns of dialogue within a single active session.
- FR15: The system can automatically apply historical constraints from the current session to new searches without requiring the user to restate them.
- FR16: The system can overwrite a specific constraint (e.g., changing the budget) while retaining all other previously stated constraints.
- FR17: The system automatically purges all conversational memory logs 30 days after session creation.

### User Account & Trip Management
- FR18: Users can initiate and complete an entire trip search session anonymously without creating an account.
- FR19: The system can prompt an anonymous user to create an account only when they explicitly choose to "Save" a trip.
- FR20: Users can register and authenticate using an Israeli phone number (+972 prefix).
- FR21: Users must explicitly confirm a consent checkbox regarding AI memory storage during the registration flow.
- FR22: Authenticated users can view a "Saved Trips Dashboard" containing all their previously saved trip packages and constraints.
- FR23: Authenticated users can re-initiate a chat session using a saved trip as the baseline context.

### Administration & Operations
- FR24: Admin users can access a secure internal dashboard (localized in Hebrew).
- FR25: Admin users can view the current health status and latency metrics for the Kiwi and Booking.com API integrations.
- FR26: Admin users can view the aggregate Click-Through Rate (CTR) for generated affiliate links.
- FR27: The system can append the correct affiliate tracking IDs to all outbound Booking.com and Kiwi URLs.
