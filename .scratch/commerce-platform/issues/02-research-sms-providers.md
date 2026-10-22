# SMS providers: which Iranian providers can send OTP, and what do their official docs say?

Type: research
Status: open
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
