# Payment gateways: which Iranian gateways fit V1, and what do their official docs say?

Type: research
Status: open
Blocked by: none

## Question

For the main Iranian online payment gateways, what do their **official** sites and docs say about each point below? Confirm the current list from official sources first. Candidates to check: Zarinpal, IDPay, Zibal, NextPay, Pay.ir, PayPing, and the direct bank gateways of Mellat (Behpardakht), Saman (SEP), Parsian and Melli (Sadad).

1. How a payment is started and verified (redirect, callback, verify call).
2. The amount unit (Rial or Toman) and any currency parameter.
3. Sandbox or test mode.
4. Refund or reversal APIs, or manual only.
5. How a callback or webhook proves it is genuine (signature, IP allowlist).
6. Hosting or IP restrictions: must the server that calls the API be in Iran?
7. Merchant onboarding requirements (legal entity, Enamad, documents) and any public fees or settlement times.
8. Where the official API documentation lives (URL) and its date or version.

## Context

- The owner chose online card payment through a gateway only, behind a replaceable adapter. Wallet, cash on delivery and installments are out of V1.
- Anything not in an official document is recorded as "docs needed". No gateway API is invented.

## Inputs

- Skill: `research` (primary sources only; cite each claim).
- Write findings to `.scratch/commerce-platform/research/payment-gateways.md`.

## Done when

The findings file has a citation per claim and a "docs needed" list. The answer lists the facts that the dependent tickets wait on.
