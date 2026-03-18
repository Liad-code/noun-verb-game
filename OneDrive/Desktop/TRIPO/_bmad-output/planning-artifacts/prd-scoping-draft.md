## Project Scoping & Phased Development

### MVP Strategy & Philosophy

**MVP Approach:** Experience MVP focused on demonstrating the core "wow" factor of conversational, context-aware travel planning for the Israeli market. The goal is to secure high-intent validation ("זה בדיוק מה שחיפשתי") rather than covering all edge cases perfectly.
**Resource Requirements:** Solo founder + AI agents (BMAD & Antigravity). Target launch in 8-10 weeks. Bootstrapped approach utilizing free-tier API limits where possible for initial validation.

### MVP Feature Set (Phase 1)

**Core User Journeys Supported:**
- Primary Happy Path (Sarah): Overwhelmed planner finding a curated flight+hotel package.
- Anonymous Discovery: Users can browse and chat without an account to minimize initial friction.
- Frictionless Conversion: Account creation prompt triggered only when a user attempts to "Save Trip".
- Admin Operations (Alex): Basic monitoring of API health and affiliate CTRs.

**Must-Have Capabilities:**
- **Strictly Scoped Routing:** One-way flights and hotels ONLY. Single departure airport (TLV - Ben Gurion) strictly enforced with a maximum of 3 curated destination suggestions per query to guarantee < 5s response time.
- **Anonymous Sessions with Delayed Auth:** Open initial chat experience; registration required only for saving a session.
- **Contextual Memory:** Real-time persistence of constraints during the active chat.
- **Saved Trips Dashboard:** Essential for proving long-term value and hitting the 30% retention metric.

### Post-MVP Features

**Phase 2 (Growth):**
- Round-trip and multi-city flight capabilities.
- Additional departure airports beyond TLV.
- Expanded travel verticals: Car rentals, localized activities.
- Price drop alerts on saved trips.

**Phase 3 (Expansion):**
- Proactive AI trip suggestions based on past travel history.
- Social group collaboration tools for planning trips together.
- Native iOS/Android applications.
- B2B White-label solutions for travel agents.

### Risk Mitigation Strategy

**Technical Risks:** Mitigated by severely constraining the MVP scope to one-way flights out of TLV and hotels, ensuring the aggregator APIs can respond within the < 5s latency requirement without complex round-trip or layover routing calculations.
**Market Risks:** Mitigated by focusing exclusively on an Experience MVP for a specific, underserved niche (Israeli travelers needing authentic conversational Hebrew). Anonymous testing reduces user acquisition friction.
**Resource Risks:** Mitigated by the Solo + AI development model, aiming for a rapid 8-10 week launch. Leveraging free-tier APIs initially to prove the affiliate CTR viability before incurring scalable infrastructure costs.
