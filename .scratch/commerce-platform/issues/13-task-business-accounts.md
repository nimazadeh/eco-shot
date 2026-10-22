# Owner accounts and test access: gateway, SMS, domain, hosting, Enamad

Type: task
Status: open
Blocked by: 01, 02, 04

## Question

After the gateway, SMS and tax research is in, give the owner a precise checklist: which gateway and SMS accounts to apply for (and the documents each needs), what to check with their accountant (VAT, Moodian, Enamad), and the domain and hosting status. Record what exists, any sandbox or test access obtained, and where credentials will live (the owner's ignored `.env.local` only; never in the repository or in chat).

## Context

- Human in the loop. Lead times for gateways, SMS lines and Enamad can be weeks; this ticket exists so they start early.
- The owner never pastes secrets into chat or the repository.
- From the Torob and Emalls research: both require an Enamad before listing; Emalls asks for further documents for phones, digital goods and computers.
- From the SMS providers research: an OTP template must be approved or created in the provider's panel before the first code is sent, and some providers need the server's outgoing IP registered. Prices were not fetched; the owner should check them at sign-up.

## Inputs

- Read first: the payment gateways, SMS providers and tax and law research.

## Done when

The owner has a checklist, and the answer records what exists, what was applied for, and any test access, without secrets.
