# SMS providers: which Iranian providers can send OTP, and what do their official docs say?

Type: research
Status: resolved
Blocked by: none

## Question

For the main Iranian SMS providers, what do their **official** sites and docs say? Confirm the current list first. Candidates: Kavenegar, Ghasedak, SMS.ir, Melipayamak, Farapayamak.

1. OTP or template sending (often called pattern or lookup) versus free-text sending.
2. Sender lines and any approval needed for OTP message content.
3. Delivery reports.
4. Sandbox or test mode.
5. Rate limits and anti-fraud features.
6. Hosting or IP restrictions: must the calling server be in Iran?
7. Public prices.
8. Official documentation URLs and their date or version.
9. Any rule from an official regulator about OTP messages (cite it, or say none was found).

## Context

- Sign-in is phone OTP only, for customers and staff, so the SMS provider is critical and stays behind a replaceable adapter. A fake provider exists for development only and refuses to run in production.
- No provider API is invented; gaps are recorded as "docs needed".

## Inputs

- Skill: `research`.
- Write findings to `.scratch/commerce-platform/research/sms-providers.md`.

## Done when

The findings file has a citation per claim and a "docs needed" list. The answer lists the facts that the dependent tickets wait on.

## Answer

Findings with a citation per claim: [research/sms-providers.md](../research/sms-providers.md) (official docs read 2026-10-22).

- **All four candidates document a template-based OTP call** (Kavenegar verify lookup, SMS.ir verify, Ghasedak OTP, Melipayamak SendOtp or pattern). Voice OTP: Kavenegar and Ghasedak. **Only SMS.ir documents a sandbox** (test key, no real SMS, no credit used).
- **OTP wording is fixed per approved template**, and entirely fixed in Melipayamak's `SendOtp`. A new template needs provider-side approval or setup first, so the first real code cannot be sent until the owner's account and template exist.
- **Provider-side IP and rate controls exist** (Kavenegar optional IP allowlist and per-IP limits, SMS.ir IP whitelisting and HTTP 429, Melipayamak allowed-IP list plus daily and volume limits). The production server's outgoing IP may need registering; changing hosting can break sending.
- **Credentials must stay out of logs.** Kavenegar puts the API key in the URL path, so URLs are secrets.
- **No official OTP rule found.** A regulator rule on informational service-line SMS exists (secondary sources); whether verification messages are exempt is unknown.
- **Docs needed:** prices, the regulator's primary text, Ghasedak IP and limits, Kavenegar sandbox and full verify page, Melipayamak sandbox and reports, SMS.ir IP and rate-limit details (listed in the findings file).
