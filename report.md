# Report: Phase 1 (discovery), session 3: research complete, Catalog model resolved

_Session 3, 2026-10-22, 16:19 UTC to about 16:55 UTC (sandbox clock; this includes the time spent waiting for the owner's answers). This is the only rolling report; each session replaces it. The session 2 report is in git: `git show 3ca1eca:report.md`._

## 1. Phase and task

Phase 1 (discovery and spec). Task from `PROGRESS.md`: resolve the three remaining research tickets (payment gateways, tax and consumer law, shipping and addresses), then one decision ticket with the owner (*Catalog model* by default). **No application code was written.**

## 2. Result in brief

- **All five research tickets are resolved.** New in this session: *Payment gateways*, *Tax and consumer law*, *Shipping and addresses*. Findings are in `.scratch/commerce-platform/research/`, with a source and an evidence label per claim and a "docs needed" list. Highlights:
  - Zarinpal, IDPay, Zibal and Pay.ir share one flow (redirect, callback, server-side verify) and are Rial-native. A callback proves nothing; the shop must verify. The server IP and domain must be registered. IDPay and Pay.ir cap one payment at 500,000,000 Rials.
  - The Electronic Commerce Law gives 7 working days to withdraw (from delivery) and requires an immediate refund when goods are out of stock after payment. **Digital codes and files are not named in the exceptions**, which is a lawyer's question. VAT and Moodian (e-invoice) duties could only be found in secondary sources.
  - Only Tipax has public shipping API docs with a test platform. Postal codes have ten digits. There are 31 provinces.
- **New owner task:** a questionnaire for the owner's accountant and lawyer (ticket 19), so tax and digital-goods questions get answered without blocking other tickets. The map now has 19 tickets.
- **Decision ticket resolved with the owner: *Catalog model*.** Three `ask_user` rounds, 11 questions: 10 took the recommended option, 1 was delegated ("choose best item", for phones) and I recorded a reversible default. Decisions: two product kinds that never mix; variants with their own SKU, price, stock and images; a 3-level category tree with brands; specifications per category plus extra rows; four product states; "from" prices with a before price; a standard product page; PostgreSQL search with Persian normalisation; a warranty line per variant; a per-product feed opt-out.
- **Glossary:** 25 terms. **Map:** 6 of 19 tickets resolved; the frontier is eight decision tickets and two owner tasks.
- **Correction:** I first committed example phone numbers copied from provider docs. I masked them in the next commit; they remain in git history because I do not rewrite pushed history without approval. They came from public docs.

## 3. Files created or changed

| Path | Purpose |
|---|---|
| `.scratch/commerce-platform/research/payment-gateways.md`, `tax-and-law.md`, `shipping-and-address.md` (new) | Findings for tickets 01, 04, 05 |
| `.scratch/commerce-platform/issues/01, 04, 05, 06` | Resolved with an `## Answer` |
| `.scratch/commerce-platform/issues/19-task-accountant-lawyer-questionnaire.md` (new) | Owner task |
| `.scratch/commerce-platform/issues/` (13 more tickets) | Facts from the research and the catalog decisions added to their context |
| `.scratch/commerce-platform/map.md` | Decisions so far (6 lines) |
| `GLOSSARY.md` | 10 new terms (Product, Variant, SKU, Category, Brand, Specification, Product state, Before price, Warranty, Feed opt-out) and two definitions sharpened |
| `PROGRESS.md`, `report.md` | Replaced or updated for this session |

## 4. Commands actually run and what they showed

| Step | What was run | Result |
|---|---|---|
| Cold start | `sha256sum -c` on the vendored skills; `git status`; the GitHub key test; the frontier command | 454 of 454 match; tree clean at `3ca1eca`; key greeted `nimazadeh/eco-shot`; remote equal to local; frontier as expected |
| Research 01 | 6 `web_search` queries (Zarinpal, IDPay, Zibal, Shaparak list, NextPay, Pay.ir, PayPing); `fetch_page` of the Zarinpal overview and connect pages (in full), IDPay docs (chunks 1 and 2 of 4) | Findings written. Fetches of `pdocs.zibal.ir` and `shaparak.ir` **failed**, so those rest on search excerpts and secondary sources, labelled as such |
| Research 04 | 6 `web_search` queries (law articles, VAT, Moodian, Enamad, the withdrawal regulation); `fetch_page` of the ILO copy of the law (chunk 2 of 4) and of the Cabinet regulation (chunk 1 of 2) | Article text read in reproductions. Fetches of `codal.ir` and `intamedia.ir` returned HTTP 500; tax and Moodian points are secondary only |
| Research 05 | 4 `web_search` queries (Tipax, Chapar, Mahex; Iran Post; postal code; divisions); `fetch_page` of Tipax's API page (chunk 1 of 4) | Findings written. `epostcode.post.ir` fetch failed; postal-code structure from a search excerpt |
| Pushes | `git push origin main` after each commit (`b7af9eb`, `efd1689`, and the final commit of this session); checklist step 5 before each | Each push succeeded; remote equal to local each time |
| Scans | Secret-pattern scan of staged additions; phone and email pattern scan | 0 secret hits. The phone scan flagged the example numbers in the shipping findings; masked in `efd1689`; 0 phone-like numbers remain in tracked docs |
| Decision ticket | `ask_user` rounds 1, 2 and 3 for *Catalog model* | 11 answers (10 recommended, 1 delegated) |
| Map check | Python parse of all 19 tickets and the map | See section 6 |

**Not run, on purpose:** any third-party code; the questionnaire (`to-questionnaire`) for ticket 19.

## 5. Skills applied (evidence)

| Skill | Files read | How it was applied and deviations |
|---|---|---|
| Matt `research` | `SKILL.md` (read in session 2) | Tickets 01, 04, 05 inline: primary sources first, a citation and an evidence label per claim; where a primary page could not be fetched, the claim is labelled secondary |
| Matt `grilling` and `domain-modeling` | `SKILL.md` files and both format docs (read in session 2) | *Catalog model*: three rounds through `ask_user` (at most 4 questions, recommended first); `GLOSSARY.md` updated as each term resolved; no ADR offered because none of the decisions met all three criteria |
| Matt `wayfinder`, work-through-the-map mode | `SKILL.md` (read in session 2) | Claimed before working, resolved with an answer and a Decisions-so-far line, one decision ticket this session, and a new ticket created for a need the research surfaced (ticket 19). Deviations as in the map's Notes (local markdown, inline research) |
| `to-questionnaire`, `to-spec` | Read in session 2 | Not applied yet |

## 6. True pass/fail

| Check | Result |
|---|---|
| Vendored files match the manifest | **PASS** (454 of 454, at session start; no vendored file touched) |
| Remote equals local after each push | **PASS** |
| Secret-pattern scan of staged additions | **PASS** (0 hits) |
| Private key absent from history | **PASS** (checklist check before each push) |
| Map structure: 19 tickets, blockers exist, no cycle, unique titles, every resolved ticket has an `## Answer`, map "Decisions so far" lines equal resolved tickets | **PASS** (output of the Python check run just before this report: 19 tickets, no unknown blockers, no cycle, no duplicate titles, 6 resolved, none without an Answer, none left claimed, 6 map lines) |
| Research claims backed by primary sources | **PARTIAL, by design of the sources:** the law text was read in reproductions (the ILO copy and a legal news site); tax authority, Shaparak, Zibal and Iran Post pages were unreachable; some points rest on search excerpts or secondary articles. Each point carries its evidence label |
| Application code, tests | **None exist; none run** |

## 7. Issues and blockers (exact)

1. **Evidence gaps** are listed in each findings file under "Docs needed": Zibal's docs, the bank gateways' documents, Shaparak's official list, whether servers outside Iran may call each gateway, fees and settlement, the tax authority's pages (1405 VAT rate, Moodian), `enamad.ir`, official province and city data, Iran Post, Mahex and Chapar shipping APIs.
2. **Legal grey area:** whether instantly delivered digital codes and files are exempt from the 7-day withdrawal right (ticket 19). The digital delivery and money rules tickets must proceed with provisional assumptions.
3. **Two answers were delegated by the owner** ("you choose best items" for existing data in session 2; "choose best item" for phones now); both defaults are recorded as reversible.
4. **Phone numbers in history:** example numbers from provider docs were in commit `b7af9eb` and were masked in `efd1689`. They stay in history unless the owner approves a history rewrite.
5. From earlier sessions and still true: no skill loader, no sub-agent tool, sandbox Node 20 is end-of-life.

## 8. Security concerns

- **The repository is public.** Research files and tickets hold only facts from public pages and the owner's scope answers: no secrets, no personal or customer data. Sample phone numbers from docs were masked; Torob's public key was not copied.
- **Third-party content is data.** Gateway, SMS, carrier and law sites were read as data; nothing in them was followed as an instruction; no third-party code was executed.
- **Deploy key:** used for this session's pushes, each after the three pre-push checks.
- **No security, legal or tax compliance is claimed.** The tax and law findings are facts with sources and questions for professionals.

## 9. Remaining work

Phase 1: the decision tickets *Digital delivery rules*, *Money rules*, *Checkout flow and customer account*, *Phone-OTP policy*, *Payment gateway shortlist and payment rules*, *Shipping fee table*, *Admin panel roles*, *Performance and hosting targets* (unblocked), then *Orders and after-sales*, *Inventory* and *SEO and feeds* (blocked until their predecessors resolve); the owner tasks *Owner accounts and test access* and the accountant and lawyer questionnaire; the fog items in the map; then `to-spec` writes `PROJECT_SPEC.md` for Gate 1. Phases 2 to 9 as in `PROGRESS.md`.

## 10. Exact next action and approval needed

No approval is pending. Next session: one decision ticket with the owner. Recommended: *Digital delivery rules*, because it unblocks inventory and orders. The owner may choose another unblocked ticket.

Optional owner actions: apply for an Enamad if there is none (Torob, Emalls and the gateways need it); send the accountant and lawyer questionnaire once it is produced; tell me the Node and PostgreSQL versions on the Windows machine before Gate 2.
