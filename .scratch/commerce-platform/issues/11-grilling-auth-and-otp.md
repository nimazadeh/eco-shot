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

## Inputs

- Skills: `grilling`, `domain-modeling`, Addy `security-and-hardening`.
- Read first: the SMS providers research.

## Done when

Each numbered item has an owner decision (or moves to the fog or out of scope), `GLOSSARY.md` is updated as terms resolve, and the answer gists the decisions for the map.
