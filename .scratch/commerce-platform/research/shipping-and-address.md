# Research: shipping, addresses and phone formats for Iran

Ticket: *Shipping and addresses: carriers, address data and phone formats for Iran.* Researched 2026-10-22 (session 3). Every claim names its source and how it was read. Gaps are listed under "Docs needed"; nothing is guessed.

Evidence labels: **[F]** read on the provider's official page; **[E]** search excerpt of an official page (page not read in full); **[S]** secondary source; **[X]** official page could not be fetched.

## Sources (retrieved 2026-10-22)

| Id | Source | How it was read |
|---|---|---|
| C1 | Tipax academy page "How to use the e-Tipax API", https://academy.tipax.ir/education/api-ای-تیپاکس/ , and `tipaxco.com/etipax` | Academy page chunk 1 of 4 [F]; the other page as an excerpt [E] |
| C2 | Mahex: https://mahex.com/ and its FAQ | Search excerpts [E] |
| C3 | Iran Post: `gnaf.post.ir/reseller/ApiService/Index/0`, `epostcode.post.ir`, `post.ir` pages | Search excerpts [E]; `epostcode.post.ir` fetch failed [X] |
| C4 | Chapar | No official page found; only third-party comparisons [S] |
| C5 | Interior Ministry, Office of Country Divisions (`moi.ir`) | Search excerpt [E]; counts from Wikipedia [S] |
| C6 | Phone formats: Kavenegar docs (`kavenegar.com/rest.html`) and IDPay docs (`idpay.ir/web-service/v1.1/`) | [F] |

## Carriers

- **Tipax (C1 [F]).** An official, documented API (version 3) with a **test platform** (`omtest.tipax.ir`) and Swagger docs. Authentication: send a username, password and API key to the token endpoint and receive an access token and a refresh token (bearer). Areas: account, addresses (address book), cities (the cities Tipax serves, with ids), customers (including the wallet balance), financial, and **orders** (create an order with sender and receiver, package type, payment type, pickup and delivery type, service id). Documented service ids include same-day ground, one-day and two-day ground, intercity express, intracity express and international. Payment types include sender credit, receiver pays on delivery, cash on delivery and wallet. The page is dated 2025-06-10.
- **Mahex (C2 [E]).** Orders go by phone, in person, or in the web panel after a cooperation contract. Its site gives 24 hours to provincial capitals and 48 hours elsewhere. No official API documentation was found.
- **Iran Post (C3 [E]).** Offers pishtaz, special, registered and parcel services and an online tariff tool on `post.ir`. Its address-services site (`gnaf.post.ir`) offers reseller APIs, among them a postal-code certificate service (valid 15 days, with a 20-digit unique id). No official shipping-order API documentation was found; one logistics vendor publishes documents for a "post system API" [S].
- **Chapar (C4 [S]).** No official page found. Third-party comparisons say it has an API.

## Postal codes, provinces, phone numbers

- **Postal code (C3 [E]).** Ten digits. The first five digits identify the geographic area, the sixth the building row within it, and the last four the place within the building. The same site says the code is unique per place and assigned by Iran Post under the national-number and postal-code law.
- **Provinces and cities (C5).** There are 31 provinces. Wikipedia gives, at the end of 1401, 476 counties, 1,170 districts, 1,431 cities and 2,736 rural districts [S]. The Interior Ministry's Office of Country Divisions is the owner of the list [E]; I did not obtain an official dataset with codes. Tipax publishes the cities it serves through its API [F].
- **Mobile numbers (C6 [F]).** Kavenegar documents that it accepts a recipient in four forms: `09121234567`, `00989121234567`, `+989121234567` and `9121234567`. IDPay documents the buyer's phone as 11 characters in three forms such as `9382198592`, `09382198592` and `989382198592`. I found no official numbering-plan document.
- **National ID.** I found no official specification of the format or checksum. Whether the shop needs it at all is a decision for the checkout ticket.

## Docs needed (unresolved; do not guess)

1. Official shipping-order API documents for Iran Post, Mahex and Chapar (only Tipax's is public).
2. An official, current province and city dataset with codes (Interior Ministry or the Statistical Centre).
3. Iran Post's own page on the postal-code structure (the fetch failed) and the terms of its address and certificate APIs.
4. An official source for the mobile numbering plan and the national ID checksum (or confirmation that the shop does not need it).
5. Carrier prices and contract terms (not fetched).

## What this means for later tickets (facts to weigh, not decisions)

- **Shipping rules.** A fee table by province and weight is workable: there are 31 provinces. City-level exceptions need a city dataset the owner approves. Tipax is the only carrier with public API docs and a test platform, so a later carrier integration would start there; carriers should sit behind an adapter.
- **Address form.** The postal code is ten digits; its structure can be checked by length and digits only, unless an Iran Post lookup is added later.
- **Phone input.** Providers accept several spellings, so the app should accept the four forms (and Persian and Arabic-Indic digits) and keep one canonical form.
