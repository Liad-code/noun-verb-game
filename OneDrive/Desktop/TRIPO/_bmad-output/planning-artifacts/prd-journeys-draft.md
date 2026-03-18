## User Journeys

### 1. Sarah: "The Overwhelmed Planner" (Primary Happy Path)
**Situation:** Sarah (32, Marketing Manager mapped from TLV) is planning a summer getaway to Lisbon for her and her partner. She’s exhausted after work and dreads the 4-6 hours she usually spends opening 15 browser tabs across Kayak, Airbnb, and Google Flights. She has a ₪6,000 budget and wants a boutique hotel. 
**Opening Scene:** She opens Tripo for the first time. The interface is clean, fully RTL, and entirely in Hebrew. She types naturally: "אני צריכה לטוס לליסבון עם בן הזוג שלי בשבוע האחרון של יולי. התקציב שלנו הוא 6,000 ש״ח ואנחנו רוצים מלון בוטיק במרכז. איזה אופציות יש לנו?" (I need to fly to Lisbon with my partner the last week of July. We have a 6,000 NIS budget and want a boutique hotel in the center. What options do we have?)
**Rising Action:** In under 15 seconds, Tripo processes this query, querying Kiwi for flights out of TLV and Booking.com for hotels. It replies in conversational Hebrew, confirming her preferences and presenting three highly curated "Trip Packages." 
**Climax:** Sarah sees the second option: a perfect boutique hotel with a rooftop bar bundled with direct flights from Ben Gurion that fit exactly into her ₪5,500 budget (displaying ILS converted prices). She clicks the "Book Flights" and "Book Hotel" buttons directly on the Hebrew UI cards within the chat. 
**Resolution:** She’s handed off to the respective affiliate sites with her details pre-filled. She finishes booking in 5 minutes. Her trip is automatically saved in her Tripo dashboard for future reference.

### 2. David: "The Family Trip Coordinator" (Complex Constraints Edge Case)
**Situation:** David (41, Father of two from Ramat Gan) is trying to plan a family vacation. He is incredibly anxious about connecting flights with a toddler and needs guaranteed family rooms or adjoining suites.
**Opening Scene:** David logs in with his +972 phone number and tells Tripo: "אני מחפש חופשה משפחתית ביעד חם בנובמבר. 2 מבוגרים, ילד בן 7 ותינוק בן שנתיים. התקציב הוא 10,000 ש״ח." (I need a family vacation somewhere warm in November. 2 adults, a 7-year-old, and a 2-year-old toddler. Budget is 10,000 NIS.)
**Rising Action:** Tripo suggests a few destinations popular with Israelis: Paphos, Dubai, and Eilat. David picks Dubai but adds, "רגע, אני חייב טיסות ישירות, לא יכולים לעשות קונקשן עם התינוק." (Wait, I absolutely need direct flights, we can't do layovers with the toddler.) He also adds, "תדאג שיש בריכה במלון." (Make sure the hotel has a pool.) Tripo immediately filters all its backend searches out of TLV. It presents direct El Al/Flydubai flights and family-friendly resorts. 
**Climax:** David realizes he needs to adjust the dates. He says, "בעצם, תדחה את זה בשבוע." (Actually, push this back by one week.) Instead of starting over, Tripo remembers the toddler, the direct flight constraint out of TLV, the pool requirement, and the ₪10,000 budget, and seamlessly updates the flight and hotel cards for the new dates (formatted DD/MM/YYYY).
**Resolution:** David experiences profound relief. He clicks the affiliate links, books the trip natively from the RTL interface, and saves this setup for next year's vacation.

### 3. Alex: "Platform Admin" (Internal Operations)
**Situation:** Alex is the Operations Lead at Tripo based in Tel Aviv. They need to ensure the core revenue engine—affiliate links and API integrations prioritizing Israeli providers—is running smoothly. 
**Opening Scene:** Alex logs into the Tripo Admin Dashboard (which is also presented in Hebrew with RTL layout) on a Sunday morning to do a routine health check.
**Rising Action:** They review the "Integration Health" dashboard. They see green lights for Kiwi and El Al, but notice a spike in latency from the Booking.com API. 
**Climax:** Alex drills into the affiliate link activity. They can see that while hotel searches are slightly delayed, the CTR (Click-Through Rate) on the generated Booking.com affiliate links remains high at 45%. They also note that flight partner Kiwi is generating 60% of current revenue from European routes. 
**Resolution:** Satisfied that the core business metrics are healthy and tracking correctly, Alex notes the Booking.com latency in a bug report for the engineering team and continues their day, confident that the platform is reliably monetizing user sessions.

### Journey Requirements Summary

**Core Capabilities Revealed:**
- **Hebrew NLP & Conversational Parsing:** Ability to extract complex parameters (dates formatted as DD/MM/YYYY, budgets in ILS or foreign currency, destinations) from natural Hebrew language.
- **RTL UI/UX Support:** Full Right-to-Left layout for all chat components, interactive cards, and dashboards.
- **Context Management Engine:** A persistent memory store that retains constraints across multiple conversational turns in Hebrew.
- **API Orchestration Layer:** Real-time querying of providers (Kiwi, Skyscanner, Booking.com) with an emphasis on origin TLV and local coverage. Must handle currency conversion to display ILS alongside source currency.
- **Dynamic UI Rendering:** Chat interface that can render interactive "Cards" containing rich media and affiliate redirect buttons.
- **User Account Management:** Secure authentication supporting Israeli phone numbers (+972) and a "Saved Trips" dashboard.
- **Admin Analytics Dashboard:** Internal tool (in Hebrew) to monitor API health, affiliate link click-through rates, and overall platform performance.
