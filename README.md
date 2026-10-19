# Event Management System (EVENTRA)
## Group Members
* **Aliansky Winston Viando** — `2902708224`
* **Nizar Ramadhan** — `2902725672`
* **Renjiro Kunio Handoko** — `2902740591`
---
## Deliverables & Media Links
* **Proposal Presentation Video**: [Watch on Google Drive](https://drive.google.com/file/d/1rzX4p-vrkjEclrJo16CmxlViSj8Hj95M/view?usp=sharing)
### Entity-Relationship Diagram (ERD)
* **Before AI Review**: [View ERD v1](https://drive.google.com/file/d/1cO2sv_LjhD-rl2w2ldWVyMzkxzFzuIOG/view?usp=drivesdk)
* **After AI Review**: [View ERD v2](https://drive.google.com/file/d/1gwqvg5HejcpKX7dbSQWqB7_U4-v81ZuT/view?usp=drivesdk)
### Normalization Documentation
* **Version 1 (Before AI)**: [View Spreadsheet](https://docs.google.com/spreadsheets/d/14fpw2edzZV8etOHjjeOs7vJ9F_022TG2ObSlos6V82Q/edit?usp=sharing)
* **Version 2 (After AI)**: [View Spreadsheet](https://docs.google.com/spreadsheets/d/14fpw2edzZV8etOHjjeOs7vJ9F_022TG2ObSlos6V82Q/edit?usp=sharing)
---
## AI Review Decision Register
### 1. Entity-Relationship Diagram (ERD)

| Component / Table | Proposal / Suggestion | Decision | Rationale / Architectural Impact |
| :--- | :--- | :--- | :--- |
| **`Waitlist` Table** | Add `ticketTypeId` foreign key. | **Accepted** | Previously, users could only register for general event availability. Tier-specific waitlisting allows attendees to queue for specific ticket categories (e.g., VIP, Early Bird) as individual tiers sell out. |
| **`Order` Table** | Add explicit `taxAmount` and `feeAmount` columns. | **Rejected** | Ticket pricing on Eventra is structured as all-inclusive to maintain clean price presentation. Payment gateways (e.g., GoPay, BCA) compute processing fees dynamically at runtime, avoiding redundant schema calculations. |
| **`Review` Table** | Update unique constraint from `(ticketId, eventId)` to `(userId, eventId)`. | **Accepted** | Tying constraints to ticket IDs enabled users purchasing multiple tickets (e.g., group bookings) to submit multiple reviews. The `(userId, eventId)` constraint enforces a single review per attendee per event. |

---
### 2. Normalization

| AI Recommendation | Decision | Rationale / Engineering Approach |
| :--- | :--- | :--- |
| **Delete derived columns that violate 3NF** | **Accepted** | Stored aggregations risk data drift when underlying rows change. All derived values are calculated programmatically at runtime instead of being persisted. |
| **Replace Ticket's `orderID`, `ticketTypeID`, and `seatID` with `orderItemID` and `eventSeatID`** | **Modified** | General admission tickets (e.g., `TTO04`, `TTO05`) do not require seated assignments. We adopted `orderItemID` and made `eventSeatID` nullable while preserving its unique constraint to prevent double-booking without requiring dummy entries. |
| **Assign dedicated user accounts (`UO07`–`UO10`) to organizers** | **Rejected** | Account distinction is a business logic constraint rather than a normalization flaw. A shared `userID` violates no structural dependencies and is enforced via application middleware and triggers. |
