# Research: payment gateways

Ticket: *Payment gateways: which Iranian gateways fit V1, and what do their official docs say?* Researched 2026-10-22 (session 3). Every claim names its source and how it was read. This is evidence for decisions, not a spec for our adapter: re-read the official docs when the adapter is built (Phase 6). Gaps are listed under "Docs needed"; nothing is guessed.

Evidence labels: **[F]** read on the provider's official docs page; **[E]** search excerpt of an official page (page not read in full); **[S]** secondary source (not official); **[X]** official page could not be fetched.

## Sources (retrieved 2026-10-22)

| Id | Source | How it was read |
|---|---|---|
| S1 | Zarinpal docs: https://www.zarinpal.com/docs/paymentGateway/ and .../connectToGateway; sandbox page https://www.zarinpal.com/docs/paymentGateway/sandBox; inquiry page .../otherMethods/Inquiry; SDK method pages | Overview and connect pages fetched in full [F]; the rest as search excerpts [E] |
| S2 | IDPay docs: https://idpay.ir/web-service/v1.1/ | Fetched chunks 1 and 2 of 4 [F] (status list and error list not read) |
| S3 | Zibal docs: https://pdocs.zibal.ir/IPG/API and https://pdocs.zibal.ir/COD/API | Search excerpts only [E]; the fetch failed [X] |
| S4 | Pay.ir docs: https://docs.pay.ir/gateway/ | Search excerpt [E] |
| S5 | NextPay: https://nextpay.org/ and its 2016 technical guide PDF | Search excerpts [E]; the guide is old |
| S6 | Shaparak (official site `shaparak.ir`) | Fetch failed [X]. The list of payment companies below comes from secondary articles [S] |
| S7 | Third-party notes on Zarinpal IP behaviour (wp-needs.com, 2026-10-03), a Zibal activation note in a GitHub issue, a how-to for the Mellat gateway (portal.ir, 2024) | Search excerpts [S] |

Not looked at: PayPing; the technical documents of the direct bank gateways (Behpardakht Mellat, Saman SEP, Parsian, Sadad); IDPay's tariff page; any gateway's fee and settlement terms.

## The four documented gateways side by side

| Point | Zarinpal (S1) | IDPay (S2) | Zibal (S3) | Pay.ir (S4) |
|---|---|---|---|---|
| Flow | `POST` request returns an `authority`; redirect the buyer to the pay page; the buyer returns to `callback_url` with `Authority` and `Status=OK` or `NOK`; call verify only when `OK` [F] | `POST /v1.1/payment` returns `id` and `link`; the buyer returns to `callback` by POST or GET (set in the dashboard); then `POST /payment/verify` with `id` and `order_id` [F] | `POST gateway.zibal.ir/v1/request` returns a `trackId`; redirect to `/start/{trackId}`; callback data arrives by GET; `POST /v1/verify` with `merchant` and `trackId` [E] | request with `api`, `amount`, `redirect`; then `pay.ir/pg/verify` with `api` and `token` [E] |
| Amount unit | Request has an optional `currency` of `IRR` or `IRT`; the verify page labels `amount` as Rials; minimum 10,000 Rials [F, E] | Rials, between 1,000 and 500,000,000 [F] | Rials [E] | Rials, between 10,000 and 500,000,000 (from its error text) [E] |
| Verify rules | First successful verify returns code 100, repeats return 101 ("already verified"); the amount must match the order [F] | Verify is required; without it the money goes back to the payer [F] | Verify call per `trackId` [E] | Duplicate or repeated transactions are an error code [E] |
| Sandbox | Change the host to `sandbox.zarinpal.com`; any 36-character merchant id works [E] | Header `X-SANDBOX: 1`: everything is simulated, and **checks on website address and IP are switched off in sandbox** [F] | A sandbox host exists for its COD API; for the gateway, a third-party package says merchant `zibal` is a sandbox terminal [E, S] | Not seen |
| Refunds | The docs menu lists "reverse transaction", "unverified transactions" and "transactions" (details not read) [E] | No refund endpoint in the documented table of contents [F] | Not read | Not read |
| Callback authenticity | No signature documented; the server-side verify is the proof [F] | No signature documented; verify is the proof [F] | Not read | Not read |
| IP and host rules | The merchant must declare the main server IP of its website to Zarinpal [F]; a third-party article says a real payment only works from the registered IP while the sandbox answered from any IP [S] | Outside sandbox the API is sensitive to website address and IP [F] | A third-party issue says callback and referer must match the registered domain and the server IP must be registered [S] | `redirect` must be on the same address as the gateway address approved at Pay.ir [E] |
| Fees | The response carries `fee_type` (payer or merchant, chosen in the panel) and `fee`; the docs list a fee-calculation method [F, E] | A tariff page exists (not fetched) [F] | Not read | Not read |
| Docs location | `zarinpal.com/docs/paymentGateway/` (API v4) | `idpay.ir/web-service/v1.1/` (also v1.0) | `pdocs.zibal.ir/IPG/API` | `docs.pay.ir/gateway/` |

**Other facts.**
- **Per-transaction ceilings exist.** IDPay and Pay.ir document a maximum of 500,000,000 Rials (50,000,000 Toman) per payment; an unofficial error list for Zarinpal mentions limits set by Shaparak [F, E, S]. Orders above the ceiling need another route.
- **Payment pages.** Secondary sources say every card payment page ends in `.shaparak.ir` [S].
- **Licensing landscape.** Secondary articles list the payment service companies approved by Shaparak (among them Asan Pardakht, Iran Kish, Behpardakht Mellat, Parsian, Saman, Pasargad, Sadad) and say Shaparak keeps a public list of "payment facilitators" with status labels at `shaparak.ir/fa/companies/pardakhtyar` [S, X]. NextPay's own site calls itself a Shaparak payment facilitator with a licence number [E].
- **Onboarding.** Secondary sources say gateways need an Enamad, a tax tracking code and identity matching, and that a bank gateway needs the documents, the account and the Enamad in one person's name [S]. None of this was confirmed on an official page.

## Docs needed (unresolved; do not guess)

1. Zibal's full official docs (refund, sandbox, IP rule); the page fetch failed.
2. The technical documents of the direct bank gateways: probably issued on contract.
3. Shaparak's official list of payment service companies and facilitators; the page fetch failed.
4. For each gateway: may a server outside Iran call the API? Only IP registration rules were seen.
5. Fees, settlement times and onboarding requirements from official pages (IDPay's tariff page was not fetched).
6. Zarinpal's reverse and unverified-transaction methods, how `currency: IRT` interacts with verify, and its per-payment ceiling.
7. IDPay's status and error lists (chunks 3 and 4); PayPing and NextPay's current docs.

## What this means for later tickets (facts to weigh, not decisions)

- **Money rules.** Every documented gateway works natively in Rials; Zarinpal alone offers Toman as an option. Torob's feed needs Toman. Keeping orders in Rials and converting at the edges avoids unit mix-ups at the gateway.
- **Payment rules.** A callback proves nothing: the adapter must verify server to server with the amount from the order, and treat "already verified" as success. Gateway-side reversals and refunds differ, which fits the owner's choice of manual refunds at first. Ceilings of 500,000,000 Rials matter for expensive electronics.
- **Business accounts.** The final domain, an Enamad and the server IP are inputs to account activation, so they come before the gateway can go live. Sandbox lets development start earlier.
- **Hosting.** The production server needs a stable outgoing IP that can be registered with the gateway and with the SMS provider.
- **Adapter shape.** The common contract across the four is small: start a payment (order id, amount, return URL, optional phone) and get a reference plus a redirect URL; verify (reference, expected amount) and get a status, a bank reference and a masked card; inquire for status.
