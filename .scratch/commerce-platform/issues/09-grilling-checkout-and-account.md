# Checkout flow and customer account

Type: grilling
Status: open
Blocked by: 04, 05

## Question

1. **Flow.** The checkout steps and where the phone OTP happens (does signing in at checkout create the Customer?).
2. **Address.** Fields for Iran (recipient, phone, province, city, address, postal code, unit) and saved addresses.
3. **Digital-only carts.** Is an address needed at all?
4. **Invoice details.** Does the buyer give a national ID or company details? (Depends on the tax research.)
5. **Order notes** and abandoned carts (kept for a signed-in Customer?).
6. **Customer account area.** Orders, codes and downloads, addresses, profile, phone number change.

## Context

- Customers sign in with phone OTP only; terms defined so far: Customer, Shipment, Store pickup.
- From the tax and law research: before ordering the buyer must see identity, address and contact of the seller, all costs, offer validity and the cancellation and after-sales terms (article 33); a separate confirmation follows on a durable medium (articles 34 and 35). From the shipping and address research: postal codes have ten digits; phone numbers come in four spellings.

## Inputs

- Skills: `grilling` and `domain-modeling`. Glossary: `GLOSSARY.md`.

## Done when

Each numbered item has an owner decision (or moves to the fog or out of scope), `GLOSSARY.md` is updated as terms resolve, and the answer gists the decisions for the map.
