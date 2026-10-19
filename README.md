# Event Management System (EVENTRA)

## Group Members

1. Aliansky Winston Viando - 2902708224
2. Nizar Ramadhan - 2902725672
3. Renjiro Kunio Handoko- 2902740591 

# Proposal Presentation Video Link (Google Drive)
[Proposal Presentation Video](https://drive.google.com/file/d/1rzX4p-vrkjEclrJo16CmxlViSj8Hj95M/view?usp=sharing)

# Before and After AI Design Review

## ERD

## Normalization

1. [Version 1 (Before AI)](https://docs.google.com/spreadsheets/d/14fpw2edzZV8etOHjjeOs7vJ9F_022TG2ObSlos6V82Q/edit?usp=sharing)

2. [Version 2 (After AI)](https://docs.google.com/spreadsheets/d/14fpw2edzZV8etOHjjeOs7vJ9F_022TG2ObSlos6V82Q/edit?usp=sharing)

## AI Review Decision Table

### Normalization

| AI Recommendation | Decision | Why? |
|---|---|---|
| Delete derived columns that break 3NF. | Accept | Stored totals can drift out of date when the rows behind them change. Each value can be calculated from other columns, so we compute it instead. |
| Replace Ticket’s `orderID`, `ticketTypeID`, and `seatID` with `orderItemID` and `eventSeatID`. | Modify | Not every ticket has a seat (general admission, TTO04 and TTO05). We kept `orderItemID`, but made `eventSeatID` nullable and unique, so no fake seats are needed and one seat still can’t be sold twice. |
| Give organizers their own user accounts (UO07–UO10), since UO01 organizes and buys for EO01. | Reject | This is a business rule, not a normalization issue. A shared `userID` breaks no key or dependency. We enforce it in application logic or a trigger. |
