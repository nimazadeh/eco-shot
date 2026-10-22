# Report: Phase 1 (discovery), session 4: Digital delivery rules resolved

_Session 4, 2026-10-22, 16:59 UTC to about 17:14 UTC (sandbox clock; includes the time spent waiting for the owner's answers). This is the only rolling report; each session replaces it. The session 3 report is in git: `git show 7e170b9:report.md`._

## 1. Phase and task

Phase 1 (discovery and spec). Task: resolve **one** decision ticket with the owner, the one recommended at the end of session 3 and agreed by the owner ("start i agree"): *Digital delivery rules: code stock, files and after-delivery policy*. **No application code was written.**

## 2. Result in brief

- **Cold-start checks passed:** 454 of 454 vendored files match, git clean at `7e170b9`, the GitHub key works, remote equals local.
- **Decision ticket resolved with the owner:** two `ask_user` rounds, 8 questions, the recommended option each time. Decisions:
  - Codes are loaded as batches (paste or CSV, duplicates rejected store-wide, a note per batch).
  - A buyer reveals a code only on the order page, as the signed-in owner, behind a logged "show code" button. The "order ready" SMS never contains a code. A code stays visible while the account exists; reveals are logged and rate-limited.
  - Staff never see a full code after loading (masked view only).
  - If payment succeeds but no code is left: cancel the line and refund immediately ("refund due"), no waiting and no substitution.
  - Files live on the server's disk outside the public folder, up to 1 GB, behind a storage adapter. Each click creates a one-hour download link; 5 downloads per file per order for 1 year; staff can reset the count.
  - After delivery: no refund except an invalid code or defective file reported within 7 days (replace or refund). **Provisional** until the lawyer answers.
- **Security input:** Addy `security-and-hardening` re-read before the questions. Abuse cases designed against: a buyer opening another buyer's order, staff or database leaks of codes, guessable download links, and "reveal then claim a refund".
- **Glossary:** 4 new terms (Code batch, Reveal, Download link, Download allowance); 29 in total.
- **Map:** 7 of 19 tickets resolved. *Orders and after-sales* and *Inventory* are now unblocked; the frontier is nine decision tickets and two owner tasks.

## 3. Files created or changed

| Path | Purpose |
|---|---|
| `.scratch/commerce-platform/issues/07-grilling-digital-delivery.md` | Resolved with an `## Answer` |
| `.scratch/commerce-platform/issues/` (7 more tickets) | Facts from the delivery decisions added to their context (checkout, orders, OTP policy, payment rules, inventory, admin panel, hosting) |
| `.scratch/commerce-platform/map.md` | One more line under Decisions so far |
| `GLOSSARY.md` | Four new terms |
| `PROGRESS.md`, `report.md` | Updated and replaced for this session |

## 4. Commands actually run and what they showed

| Step | What was run | Result |
|---|---|---|
| Cold start | `sha256sum -c` on vendored skills; `git status`; GitHub key test; frontier command; claim of ticket 07 | All as expected (see section 2); ticket 07 set to claimed |
| Security input | `cat` of the full `security-and-hardening/SKILL.md` (17.5 KB) and of the sections on access control, sensitive data exposure, file upload safety and rate limiting in `references/hardening-patterns.md` | Read |
| Decision ticket | `ask_user` rounds 1 and 2 | 8 answers, all the recommended option |
| Checks | Python parse of all 19 tickets and the map; phone and secret pattern scans of staged additions | Results in section 6 |
| Push | `git commit` and `git push origin main`; checklist step 5 first | See section 6 |

## 5. Skills applied (evidence)

| Skill | Read | How it was applied and deviations |
|---|---|---|
| Matt `grilling` and `domain-modeling` | Read in session 2 | Two rounds through `ask_user`; the glossary was updated as terms resolved; no ADR offered because none of the decisions met all three criteria |
| Matt `wayfinder`, work-through-the-map mode | Read in session 2 | One decision ticket claimed, resolved with an answer and a Decisions-so-far line; facts passed to dependent tickets |
| Addy `security-and-hardening` | `SKILL.md` in full; `references/hardening-patterns.md` lines 62-81, 107-120, 186-203, 239-272 (the rest of that file and `references/security-checklist.md` not read this session) | Used for threat modelling and abuse cases during the grilling, not for reviewing code (none exists). Its "ask first" list is why file upload handlers and storing codes are flagged for the owner's approval at build time. The rate-limiting advice assumes a shared store; here it is PostgreSQL counters, per the owner's no-Redis rule |

## 6. True pass/fail

| Check | Result |
|---|---|
| Vendored files match the manifest | **PASS** (454 of 454, at session start) |
| Map structure: 19 tickets, blockers exist, every resolved ticket has an answer, none left claimed, map lines equal resolved tickets | **PASS** (7 resolved, 7 map lines) |
| Secret and phone-number scans of staged additions | Run before the commit; result in the commit step (the commit aborts on a hit) |
| Private key absent from history; remote equals local after the push | Checked by the commit step before and after the push |
| Application code, tests | **None exist; none run** |

## 7. Issues and blockers (exact)

1. **The after-delivery refund policy is provisional.** The law does not say whether delivered digital goods can be withdrawn from. The accountant and lawyer questionnaire (an owner task) asks it. Until then the 7-day invalid-or-defective rule stands.
2. **Build-time approvals:** adding file upload handlers and storing codes (a new category of sensitive data) need the owner's approval when they are built, per the security skill's "ask first" list.
3. **Design left for Phase 2:** codes stored encrypted with keys outside the database, an append-only audit log, upload checks (type allowlist, size cap, content check).
4. **Deliberately deferred to other tickets:** stock reservation during checkout (inventory), SMS wording and the support channel (orders), step-up sign-in for reveals (OTP policy), staff permissions (admin panel), retention of logs and codes (hosting and privacy targets).
5. From earlier sessions and still true: no skill loader, no sub-agent tool, sandbox Node 20 is end-of-life.

## 8. Security concerns

- **Codes are money-like assets.** This ticket only sets rules; nothing is built. The rules cover: authorization on every reveal and download, hidden until payment is verified, masked staff view, logging, rate limits, short-lived links, and encrypted storage as a Phase 2 requirement. No security or legal compliance is claimed.
- **The repository is public.** Tickets and findings contain no codes, secrets, or personal data.
- **Deploy key:** used for this session's push after the three pre-push checks.

## 9. Remaining work

Phase 1: the decision tickets *Money rules*, *Checkout flow and customer account*, *Orders, cancellations, returns, warranty, support and SMS notifications*, *Phone-OTP policy*, *Payment gateway shortlist and payment rules*, *Shipping fee table*, *Inventory and stock rules*, *Admin panel roles*, *Performance and hosting targets* (unblocked), then *SEO, Torob and Emalls feeds, and analytics* (blocked on *Money rules*); the owner tasks *Owner accounts and test access* and the accountant and lawyer questionnaire; the fog items in the map; then `to-spec` writes `PROJECT_SPEC.md` for Gate 1. Phases 2 to 9 as in `PROGRESS.md`.

## 10. Exact next action and approval needed

No approval is pending. Next session: one decision ticket with the owner. Recommended: *Money rules* (units, price display with or without VAT, discount codes), because checkout and the feeds depend on it. The owner may choose another unblocked ticket.

Optional owner actions, unchanged: apply for an Enamad if there is none; send the accountant and lawyer questionnaire once it is produced; tell me the Node and PostgreSQL versions on the Windows machine before Gate 2.
