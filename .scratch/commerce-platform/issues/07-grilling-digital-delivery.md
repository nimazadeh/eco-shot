# Digital delivery rules: code stock, files and after-delivery policy

Type: grilling
Status: resolved
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

## Answer

Owner decisions, two `ask_user` rounds on 2026-10-22 (8 questions, the recommended option each time). Security input: Addy `security-and-hardening`, re-read in full.

**Code stock**
- **Loading.** Staff paste codes or upload a CSV, one code per row. Duplicates are rejected across the whole store. Each upload is a *code batch* saved with a note (supplier, date), so a bad batch can be pulled back.
- **The buyer sees a code** only on the order page, only if signed in as the owner of the order, behind a "show code" button. Each *reveal* is logged. An SMS says the order is ready and never contains the code.
- **How long a code stays visible.** Always, while the customer account exists. Every reveal is logged and rate-limited per order.
- **Staff never see a full code after loading.** The panel shows masked codes (first and last characters), who bought each one and when it was revealed or downloaded. Support replaces a code without reading it.

**When stock runs out after payment.** Cancel that line and refund immediately: staff are alerted and the order shows "refund due". No waiting and no substitution. This matches article 39 of the Electronic Commerce Law. The refund itself is done by staff outside the system. Holding stock during checkout is decided in the inventory ticket.

**Files**
- **Storage.** On the server's disk, outside the public web folder, up to 1 GB per file, behind a storage adapter so object storage can replace it later. Uploads are checked for type, size and content; backups must include the folder.
- **Links.** A fresh *download link* is created each time the signed-in buyer clicks "download" and works for one hour. Each file can be downloaded up to 5 times per order for 1 year (the *download allowance*); staff can reset the count.

**After delivery (provisional until the lawyer answers).** No refund once a code is revealed or a file is downloaded, except an invalid code or a defective file reported within 7 days: staff check, then replace the code from stock or refund. The policy must be shown to the buyer before paying (article 33). The accountant and lawyer questionnaire asks whether the law allows this.

**Rules that follow (not separate questions)**
- A code is assigned and can be revealed only after the gateway verification has succeeded.
- A digital variant's stock is its unsold codes. A file-only variant has no stock limit. A variant with both a code and a file is limited by its codes.
- Every reveal and download checks that the signed-in customer owns that order line (the IDOR rule), and is rate-limited with counters in PostgreSQL (no Redis).
- Reveal and download events go to an audit log. Logs never contain codes or download links.

**For Phase 2 (architecture and security), not decided here:** codes stored encrypted with keys kept outside the database; the audit log append-only; upload checks (type allowlist, size cap, content check). Adding file upload handlers and storing codes (a new kind of sensitive data) need the owner's approval when they are built.

**Moved to other tickets:** reservation of stock during checkout (inventory); the "order ready" SMS wording, the support channel and how a buyer reports a problem (orders and after-sales); step-up sign-in for revealing a code (phone-OTP policy); staff permissions for batches, count resets and logs (admin panel); retention of logs and codes (performance and hosting targets).

Glossary terms added: Code batch, Reveal, Download link, Download allowance. No ADR offered: none of the decisions met all three criteria.

