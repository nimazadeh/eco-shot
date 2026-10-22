# Payment gateways: which Iranian gateways fit V1, and what do their official docs say?

Type: research
Status: resolved
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

## Answer

Findings with a citation and an evidence label per claim: [research/payment-gateways.md](../research/payment-gateways.md) (official docs read 2026-10-22).

- **Zarinpal, IDPay, Zibal and Pay.ir share one flow:** start a payment, redirect the buyer, the buyer returns to a callback URL, the shop verifies server to server. All are Rial-native; only Zarinpal offers Toman as an option.
- **A callback proves nothing.** No gateway documents a signature. Verify with the amount from the order; Zarinpal answers a repeat verify with "already verified" (101), IDPay returns the money to the payer if the shop never verifies.
- **Sandboxes:** Zarinpal (switch the host) and IDPay (a header; address and IP checks are off in sandbox). Zibal and Pay.ir not confirmed.
- **IP and domain:** Zarinpal requires the merchant's main server IP to be registered; IDPay is sensitive to website address and IP outside the sandbox. A stable outgoing IP and the final domain are needed before activation.
- **Ceilings:** IDPay and Pay.ir document 500,000,000 Rials per payment (50,000,000 Toman). Expensive electronics may exceed it.
- **Refunds:** IDPay documents no refund endpoint; Zarinpal lists a reverse method (details unread). This fits the owner's manual-refund choice.
- **Docs needed:** Zibal's full docs, the bank gateways' documents (contract-gated), Shaparak's official list, whether servers outside Iran may call each API, fees and settlement, PayPing and NextPay.
