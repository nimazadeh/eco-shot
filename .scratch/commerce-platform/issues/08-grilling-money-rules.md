# Money rules: Rial and Toman, tax display and discount codes

Type: grilling
Status: open
Blocked by: 01, 03, 04

## Question

1. **Units.** The unit stored, the unit shown to customers, and rounding. The master rule is: integers only.
2. **Tax display.** Are shown prices final (VAT included) or plus VAT? Is the shipping fee taxed?
3. **Discount codes.** Percent or fixed amount; per-order and per-customer limits; expiry; minimum order; stacking with sale prices; digital versus physical products.
4. **Gateway amounts.** The unit and conversion used when talking to a gateway.
5. **Structured data and feeds.** How prices are expressed in Product JSON-LD (ISO 4217 needs a currency code such as IRR) and in the Torob and Emalls feeds.

## Context

- Writes the explicit Rial/Toman rule that `TECH_STACK.md` must hold.
- Waits for the facts from the payment gateways, Torob and Emalls, and tax and law research.
- From the Torob and Emalls research: Torob's Product API v3 takes integer Toman and wants one all-in price per option with shipping as the only checkout extra. Emalls does not state its price unit.

## Inputs

- Skills: `grilling` and `domain-modeling`. Glossary: `GLOSSARY.md`.

## Done when

Each numbered item has an owner decision (or moves to the fog or out of scope), `GLOSSARY.md` is updated as terms resolve, and the answer gists the decisions for the map.
