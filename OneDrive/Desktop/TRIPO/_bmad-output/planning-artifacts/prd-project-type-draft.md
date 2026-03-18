## Web App Specific Requirements

### Project-Type Overview
Tripo will be built as a Single Page Application (SPA) leveraging modern JavaScript frameworks (React/Next.js). Given the target demographic (Israeli travelers), legacy browser support is not required. The user interface will be characterized by a real-time conversational flow, demanding high frontend interactivity, robust state management, and strict Right-to-Left (RTL) Hebrew localization requirements. 

### Technical Architecture Considerations

- **Application Framework:** Next.js (React) for robust component-based architecture and state management. Next.js natively supports i18n configurations required for maintaining strict RTL Hebrew layouts and routing.
- **Browser Support Matrix:** Evergreen modern browsers only (Chrome, Safari, Edge, Firefox - latest 2 versions). Mobile-responsive web views are critical as Israeli users heavily rely on mobile devices.
- **SEO & Rendering Strategy:** Minimal Server-Side Rendering (SSR) reserved strictly for the landing/marketing pages for indexing. The core application (the conversational chat interface) will be client-side rendered and excluded from search engine indexing to protect user session privacy.
- **Real-Time Data Streams:** Implementation of WebSockets for the chat AI interface to provide a low-latency, streaming response effect (similar to ChatGPT). Traditional REST architecture will be utilized for fetching aggregator API payloads (flights/hotels) which only occur once per discrete search query.

### Implementation Considerations

- **Accessibility (WCAG 2.1 AA Target):** The entire Hebrew RTL interface must guarantee full keyboard navigability. High color-contrast adherence (AA standards) is required. All dynamic and interactive "Action Cards" (affiliate Booking/Flight links) must utilize precise Hebrew ARIA labels for assistive technologies and screen readers.
