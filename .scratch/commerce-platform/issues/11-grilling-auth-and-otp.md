# Phone-OTP policy, staff extra authorization and SMS provider shortlist

Type: grilling
Status: open
Blocked by: 02

## Question

1. **OTP rules.** Code length, expiry, attempts, resend cooldown, per-phone and per-IP limits (counters in PostgreSQL, no Redis).
2. **Sessions.** Lifetime for customers and for staff.
3. **Staff extra authorization.** What fits a phone-OTP-only system (for example a second OTP for sensitive actions, allowlisted devices, owner approval for role changes)?
4. **Phone number change and lost SIM** for customers and for staff.
5. **SMS providers.** The shortlist, and the replaceable adapter contract; the development-only fake provider refuses to run in production.
6. **Abuse cases.** SMS pumping, account enumeration, replay.

## Context

- Replaces the password guidance in Addy's security skill with an OTP threat model (see `skills/SKILL_USAGE_MAP.md` section 7).
- From the SMS providers research: templates are approved per provider and fixed in wording; providers rate-limit by IP and some require an IP allowlist; only SMS.ir documents a sandbox; the regulator rule for verification messages is unconfirmed; Kavenegar's key sits in the URL, so URLs must not be logged. Voice OTP is available at Kavenegar and Ghasedak.
- From the shipping and address research: SMS and gateway providers accept phone numbers in four spellings (`09...`, `0098...`, `+98...`, `9...`), so the app should normalise input (including Persian and Arabic-Indic digits) to one canonical form.
- From the digital delivery decisions: revealing a code and creating a download link are actions of the signed-in owner of the order; decide whether such sensitive actions need a recent sign-in (step-up).

## Inputs

- Skills: `grilling`, `domain-modeling`, Addy `security-and-hardening`.
- Read first: the SMS providers research.

## Done when

Each numbered item has an owner decision (or moves to the fog or out of scope), `GLOSSARY.md` is updated as terms resolve, and the answer gists the decisions for the map.
