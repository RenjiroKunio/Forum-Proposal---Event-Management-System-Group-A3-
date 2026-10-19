# Event Management System (EVENTRA)

## Group Members

1. Aliansky Winston Viando - 2902708224
2. Nizar Ramadhan - 2902725672
3. Renjiro Kunio Handoko- 2902740591 

# Proposal Presentation Video Link (Google Drive)
[Proposal Presentation Video](https://drive.google.com/file/d/1rzX4p-vrkjEclrJo16CmxlViSj8Hj95M/view?usp=sharing)

# Before and After AI Design Review

## Entity-Relationship Diagram
1. Before AI: https://drive.google.com/file/d/1cO2sv_LjhD-rl2w2ldWVyMzkxzFzuIOG/view?usp=drivesdk
2. After AI: https://drive.google.com/file/d/1gwqvg5HejcpKX7dbSQWqB7_U4-v81ZuT/view?usp=drivesdk

## Normalization

1. [Version 1 (Before AI)](https://docs.google.com/spreadsheets/d/14fpw2edzZV8etOHjjeOs7vJ9F_022TG2ObSlos6V82Q/edit?usp=sharing)

2. [Version 2 (After AI)](https://docs.google.com/spreadsheets/d/14fpw2edzZV8etOHjjeOs7vJ9F_022TG2ObSlos6V82Q/edit?usp=sharing)

## AI Review Decision Table

### Entity Relationship Diagram
# 1. Waitlist Module (`Waitlist` Table)
* **Status**: Accepted & Implemented
* **Change**: Added `ticketTypeId` as a Foreign Key referencing `TicketType(id)`.
* **Notes**: Previously, users could only register for general event availability. Supporting tier specific waitlisting allows attendees to line up for specific ticket categories (e.g., VIP, Early Bird, General Admission) when those tiers reach capacity.

### 2. Order Details (`Order` Table)
* **Status**: Rejected
* **Decision**: Rejected the suggestion to add explicit `taxAmount` and `feeAmount` fields. Retained direct `subtotal`, `discountAmount`, and `totalAmount` attributes.
* **Notes**: Ticket pricing on Eventra is structuralized as all inclusive to maintain a clean price presentation. External payment gateways (e.g., GoPay, BCA) compute and record processing/merchant fees at transaction runtime, eliminating the need to duplicate extra fee calculations inside the internal database.

### 3. Event Feedback (`Review` Table)
* **Status**: Accepted & Implemented
* **Change**: Removed the legacy unique constraint `(ticketId, eventId)` and applied `(userId, eventId)`.
* **Notes**: Tying review constraints to ticket IDs enabled single accounts purchasing multiple tickets (e.g., group bookings) to submit multiple reviews for a single event. Enforcing a `(userId, eventId)` composite unique constraint prevents rating manipulation and ensures each user account is limited to one review per attended event.



### Normalization

| AI Recommendation | Decision | Why? |
|---|---|---|
| Delete derived columns that break 3NF. | Accept | Stored totals can drift out of date when the rows behind them change. Each value can be calculated from other columns, so we compute it instead. |
| Replace Ticket’s `orderID`, `ticketTypeID`, and `seatID` with `orderItemID` and `eventSeatID`. | Modify | Not every ticket has a seat (general admission, TTO04 and TTO05). We kept `orderItemID`, but made `eventSeatID` nullable and unique, so no fake seats are needed and one seat still can’t be sold twice. |
| Give organizers their own user accounts (UO07–UO10), since UO01 organizes and buys for EO01. | Reject | This is a business rule, not a normalization issue. A shared `userID` breaks no key or dependency. We enforce it in application logic or a trigger. |
