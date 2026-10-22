# Torob and Emalls: what do they officially require, and where are their docs?

Type: research
Status: resolved
Blocked by: none

## Question

Find the **official** documentation for stores that want their products listed on Torob and on Emalls.

1. Where the official seller or integration docs live (URL, date or version).
2. The integration method (feed, API, sitemap or crawl), the required product fields, the price unit and the update frequency.
3. Authentication, verification or onboarding steps.
4. Requirements on the store's own product pages.
5. Anything not public: record "docs needed" and who to ask. Never invent a format.

## Context

- The owner brief says the Torob and Emalls feeds are built only from official format documentation.
- If the docs are not public, the next step is a questionnaire for their support or sales contact (`to-questionnaire`).

## Inputs

- Skill: `research`.
- Write findings to `.scratch/commerce-platform/research/torob-emalls.md`.

## Done when

The findings file has a citation per claim and a "docs needed" list. The answer lists the facts that the dependent tickets wait on.

## Answer

Findings with a citation per claim: [research/torob-emalls.md](../research/torob-emalls.md) (sources read 2026-10-22; Torob docs pinned at commit `5d301d3`, 2026-10-17).

- **Torob has an official written API.** Product API v3 (the shop implements a JSON `POST` endpoint; requests carry an EdDSA-signed JWT; **prices are integer Toman**) plus a webhook, order tracking and a Shop API. It is meant for large shops and shop builders. Small shops are simply crawled after registration. Tokens come from Torob support.
- **Emalls has no public written spec for custom shops.** It registers shops by form and phone, then its robot crawls the shop. Its official WordPress/WooCommerce plugin shows the data fields it wants but is not a spec, and it never states the price unit.
- **Both require an Enamad** before listing. Emalls asks for more documents for phones, digital goods and computers.
- **Torob's terms constrain the shop:** price and availability readable with JavaScript off, one all-in price per option (shipping the only checkout extra), always-fresh price and stock, dispatch within 3 business days by default, order number and exact invoice sent to the buyer's email and mobile, tracking by order number, and a prohibited-products list that includes antivirus software.
- **Docs needed:** Emalls' written spec for custom shops; Torob's confirmation of the docs location and token process; fees (not fetched). Listed in the findings file.
