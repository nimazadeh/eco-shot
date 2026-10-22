# Digital delivery rules: code stock, files and after-delivery policy

Type: grilling
Status: open
Blocked by: 06

## Question

1. **Code stock.** How do staff load codes (one by one, bulk paste, CSV)? Duplicates? Stock per variant? Reservation during checkout? What does the buyer see, and where (order page, SMS notice)? Limits on revealing or re-showing a code?
2. **Delivery trigger.** Instant after payment is confirmed. What happens if the code stock runs out after payment: refund, wait, or substitute?
3. **Downloadable files.** Where files live, maximum size, link expiry, download count limit, re-download from the customer account.
4. **After delivery.** Refund policy for digital goods, replacement of an invalid code, the support flow.
5. **Protection.** What stops code theft and file leaks (codes hidden until payment is confirmed, an audit log of reveals and downloads)?

## Context

- The owner chose automatic delivery of both codes and files.
- Terms already defined: Digital code, Code stock, Downloadable file, Digital delivery. Consult Addy `security-and-hardening` for abuse cases.
- From the tax and law research: if the code stock runs out after payment, article 39 of the Electronic Commerce Law requires an immediate refund unless the buyer agrees to wait. Whether delivered digital codes and files fall under the withdrawal exceptions is not stated in the law or regulation (see the accountant and lawyer questionnaire); decide provisionally.
- From the catalog model decisions: a digital product delivers a code, a file or both; each variant carries its own code stock; a product never mixes physical and digital, so an order with both splits into a shipment and a digital delivery.

## Inputs

- Skills: `grilling` and `domain-modeling`. Glossary: `GLOSSARY.md`.

## Done when

Each numbered item has an owner decision (or moves to the fog or out of scope), `GLOSSARY.md` is updated as terms resolve, and the answer gists the decisions for the map.
