# Research: SMS providers for OTP

Ticket: *SMS providers: which Iranian providers can send OTP, and what do their official docs say?* Researched 2026-10-22 (session 2) from the providers' own documentation. Every claim names its source. This is evidence for decisions, not a spec for our adapter: re-read the official docs when the adapter is built (Phase 5). Gaps are listed under "Docs needed"; nothing is guessed.

## Sources (retrieved 2026-10-22)

| Id | Source | How it was read |
|---|---|---|
| S1 | https://kavenegar.com/rest.html (Kavenegar REST docs) | Fetched chunk 1 of 11. The OTP, template and return-code facts come from search excerpts of the same page |
| S2 | https://sms.ir/rest-api/ (SMS.ir REST docs) | Fetched chunks 1 and 2 of 5; the Sandbox text comes from a search excerpt of the same page; the IP whitelisting section was seen only as a heading |
| S3 | https://doc.ghasedak.me/ (overview) and https://doc.ghasedak.me/otp-resend (OTP method) | Overview fetched in full; OTP page chunk 1 of 3 |
| S4 | https://www.melipayamak.com/api/sendotp/ (page marked "last updated 17 Mordad 1405", about 2026-08-08) | Fetched in full |
| S5 | A regulator statement reported by Mehr News and Aftab News (2025-05-18); Melipayamak's rules page (excerpt) | Secondary sources; the regulator's own text was not fetched |

## What each provider's official docs say

**Kavenegar (S1).**
- REST over HTTPS, GET or POST, JSON or XML. The API key is part of the URL path: `https://api.kavenegar.com/v1/{API-KEY}/<scope>/<method>.<format>`. Consequence: request URLs are secrets and must never be logged.
- OTP is the "verify lookup" method: `receptor`, `token` (the code), more optional tokens, a `template` name and a `type` of `sms` or `call` (voice). Templates are managed through an API and carry an approval status (Rejected, PendingReview, Approved), a main send method (SMS or voice call) and a fallback method.
- Limits stated: at most 200 recipients per send call; status checks take at most 500 ids; a status-check method and a cancel method exist; calls may be made in parallel.
- **IP controls:** an optional IP setting in the panel's security settings (needed for some methods and for international lines). Return codes: 416 (calling IP does not match the settings), 429 (IP restricted), 451 (too many calls in a time window, IP restricted).
- Numbers are accepted as `0912...`, `+98912...`, `0098912...` or `912...`.
- Signing up gives a small test credit; no separate sandbox was seen.

**SMS.ir (S2).**
- `POST https://api.sms.ir/v1/send/verify` with header `X-API-KEY` and a JSON body: `Mobile`, `TemplateId`, `Parameters` (name and value pairs). The template is created first in the panel's fast-send section.
- The docs call this a service-line method, suited to verification codes and order invoices, sent within seconds, able to reach people who blocked advertising SMS. Plain messages use `POST /v1/send/bulk` from the shop's own line (at most 100 numbers per call; scheduled sends 1 hour to 365 days ahead).
- **Sandbox (documented):** a separate sandbox API key type, same URLs as production, simulated sends (no real SMS, no credit used), and one default verify template (id `123456`, text "your verification code: #CODE#"). The docs say it exists for development before a site is complete, for example before an Enamad exists.
- **Rate limiting and IP:** HTTP status codes are 200, 400, 401, 429 ("unauthorized number of requests") and 500; the limit values were not read. A section titled security settings and IP whitelisting exists; its content was not read. A reports section exists.
- Times are Unix time in UTC. An older SMS.ir page (2023) says the fast-send module has a fee and is renewed yearly; check the current terms.

**Ghasedak (S3).**
- Base `https://gateway.ghasedak.me/rest/api/v1`; HTTPS only; API key in an `ApiKey` header; JSON in and out.
- OTP: `POST /webservice/sendotpsms` with `receptors` (each with a `mobile` and a `clientReferenceId`), `templateName`, named `inputs`, optional `sendDate` and `isVoice`. An older method (`sendotpwithparams`) takes `param1`..`param10`.
- Documented behaviour: the OTP service is for verification and system messages; businesses without their own service line can use Ghasedak's lines; with its own line, the message goes there first and falls back automatically to an alternate route (text or voice) on delay or fault; a second request for the same number within 2 minutes goes by the alternate route. Voice OTP to mobiles and registered landlines, and international SMS to over 200 countries, are offered.
- The response lists, per receptor, `messageId`, `cost`, `clientReferenceId` and `messageBody`, plus `totalCost`.
- No sandbox, IP rule or rate limit was seen in what I read.

**Melipayamak (S4).**
- `SendOtp` takes a username and password, `from`, `to` and `code`; the message text is fixed ("your verification code: ..."), so it cannot carry custom wording. A pattern route (`SendByBaseNumber` and `SendByBaseNumber2` with a `bodyId` approved by the provider) allows custom approved text.
- The return codes show the account controls: a required allowed-IP list (109), a mandatory API key instead of a password (110), temporary IP blocking after failed attempts (108), an invalid calling IP (111), a daily sending limit (3), a volume limit (4), sending from public lines through the web service not allowed (9), links in the text refused (14), a recipient on the telecom blocked list (35 in REST).

## Patterns across the four (facts, not decisions)

- All four document a template-based verification or OTP call. Voice OTP is documented at Kavenegar and Ghasedak. Only SMS.ir documents a sandbox.
- Credentials differ: key in the URL path (Kavenegar), header key (SMS.ir, Ghasedak), username with password or key (Melipayamak). An adapter must keep all of them out of logs, including URLs.
- Templates need approval or prior setup in the provider's panel before the first code can be sent, so the OTP wording is fixed per template (and fixed entirely in Melipayamak's `SendOtp`).
- Providers limit by IP. Kavenegar, SMS.ir and Melipayamak offer or require an IP allowlist; Kavenegar and SMS.ir return 429-style errors for too many calls; Melipayamak has daily and volume limits. The production server's outgoing IP may therefore need registering, and a hosting change could break sending.
- Ghasedak returns a per-message id and accepts a client reference id, which suits tracking and de-duplication; the others return message ids too (SMS.ir and Kavenegar in their responses).

## Regulator (secondary sources only, S5)

A regulator official said that, under resolution 3-270 ("rules governing SMS content services"), mass advertising, informational and value-added SMS without prior consent is forbidden, and that senders of informational SMS from service numbers must end the message with the Persian "cancel 11" text. Melipayamak's rules page repeats the "cancel 11" requirement, and its API refuses multi-recipient sends without it. **Whether single one-time verification messages are exempt is not stated in what I found.** The regulator's own text must be read before the OTP template wording is fixed.

## Docs needed (unresolved; do not guess)

1. **Prices** at every provider (per message, per service line, yearly module fees): no price page was fetched.
2. **Regulator's primary text** (resolution 3-270 or its current replacement) and any OTP-specific rule.
3. **Ghasedak:** IP restrictions, rate limits and any test mode (remaining chunks of the OTP page and the overview were not read).
4. **Kavenegar:** the full verify-lookup page (10 of 11 chunks unread), sandbox beyond the sign-up credit, and the exact status-check fields.
5. **Melipayamak:** sandbox, delivery reports and the pattern approval process.
6. **SMS.ir:** the IP whitelisting section and the rate-limit values.
7. Farapayamak and any provider the owner already uses (not looked at).
