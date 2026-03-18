## Domain-Specific Requirements

### Compliance & Regulatory
- **Israeli Consumer Protection Law (חוק הגנת הצרכן):** All results must include a clear Hebrew disclaimer indicating Tripo is an aggregator: "טריפו הינה פלטפורמת השוואה. ההזמנה מתבצעת ישירות מול הספק" (Tripo is a comparison platform. Booking is made directly with the provider).
- **Privacy & Data Retention:** Conversation logs stored for a maximum of 30 days before auto-deletion. Explicit consent checkbox required during user signup for AI memory storage.
- **PII Restrictions:** No sensitive PII (passports, national IDs) may be stored under any circumstances; only trip preferences and search historical context.

### Technical Constraints
- **Currency Handling:** Exchange rates for ILS (₪) must be refreshed every 24 hours. There is to be a maximum acceptable margin of error of ±2% on the ILS converted prices shown to users.
- **RTL Integrity:** Strict Hebrew UI validation. All API text must be rendered in Hebrew-safe UI containers, applying LTR overrides only for numerical data, prices, and airport/airline codes.

### Integration Requirements
- **API Sourcing:** Must utilize official partner APIs exclusively (no web scraping allowed) to prevent IP blocking.
- **Rate Management:** Implement request queuing capped at 3 concurrent API calls to the providers.
- **Caching Strategy:** Cache common search routes and combinations for 15 minutes to reduce API loads while maintaining < 5s latency.

### Risk Mitigations
- **AI Hallucinations:** The conversation engine is strictly forbidden from generating fictional flights, hotels, or prices. It must ONLY display results explicitly successfully returned from live API calls containing valid affiliate URLs. In the event of zero API results, the AI must communicate this limitation honestly in Hebrew.
