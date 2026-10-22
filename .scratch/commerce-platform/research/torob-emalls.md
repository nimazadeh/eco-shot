# Research: Torob and Emalls

Ticket: *Torob and Emalls: what do they officially require, and where are their docs?* Researched 2026-10-22 from primary sources only. Every claim names its source. This file is evidence for decisions, not a spec for our code: before building a feed or endpoint (Phase 7), re-read the cited source at that time. Nothing is invented; gaps are listed under "Docs needed".

## Sources

| Id | Source | Retrieved | Notes |
|---|---|---|---|
| S1 | `github.com/torob/Torob-Sync` ("Torob Shop Integration & API Documentation"), commit `5d301d33abc1ae58fc8a37227c87a5bdb1173aac`, 2026-10-17 | 2026-10-22, cloned and read | Files read in full: Readme, Product API v3, Token guide, Product webhook. Skimmed (headings and intro only): Product API v1 and v2, Order tracking, Action tracking, Shop API, Shop generator connection |
| S2 | https://torob.com/sell/ | 2026-10-22 | Torob's own seller registration page |
| S3 | https://panel.torob.com/s/terms | 2026-10-22, all 3 chunks | "Terms of cooperation" (binding contract between Torob and shops) |
| S4 | https://emalls.ir/ShopRequest/ | 2026-10-22, first of 4 chunks | Emalls registration page and FAQ |
| S5 | https://wordpress.org/plugins/emalls-extraction-api-official/ and its package `emalls-extraction-api-official.1.3.0.zip` (sha256 `38a8a565dd721e2f0ce39d100e25eccf67c2e7914b91a20e86fb5bc1de8c08e8`) | 2026-10-22 | Official plugin by "Emalls" (MIT). Source read as data, never executed |

Not fetched: `panel.torob.com/s/shopIntegrationGuide`, `/faq`, `/tariff`; the rest of S4; Emalls' terms page. A search result from Torob's seller blog said an independent domain is required and builder subdomains are refused, but its page returned no text when fetched, so that statement is **unverified**.

**Provenance caveat for S1.** GitHub organization `torob` hosts it and describes it as Torob's integration documentation, and its newest commit is from 2026-10-17. I found no link to it from torob.com in the pages I fetched. Ask Torob support to confirm it is the official location before relying on it.

## Torob

**How a shop gets listed (S2, S3).**
- An online shop needs an Enamad (e-trust symbol) before joining, and must renew it if it expires (S3).
- Steps (S2): Enamad, registration form, review by Torob staff within 48 hours (result by SMS), charge the panel account, Torob's robots read the product data (up to a week), Torob's content team categorizes and merges products (up to another week), then products appear.
- Torob charges no sales commission; the customer pays the shop directly (S2). Paid click plans and a tariff page exist (S2); prices were not fetched.
- A public profile page is created for every shop and stays public after the shop leaves (S3).
- Torob may ask for identity documents (national ID image, a video of the Enamad owner, address proof) (S3).

**Two integration routes (S1 Readme).**
1. **Crawling.** Small shops are added automatically after registration and do not need to implement the APIs (S1, S2).
2. **APIs for large shops and shop builders (S1).** Product API v3 is current; v2 and v1 are deprecated. Also: a Product Webhook, an Order Tracking API, an Action Tracking API and a Shop API (read products and their Torob status, switch products on or off, read stats and clicks, answer price reports; a shop builder must be connected by each shop's approval). API tokens and credentials come from Torob support (S1 Readme, S1 Product webhook).

**Product API v3, as documented in S1 (what a shop implements).**
- Torob calls a `POST` endpoint on the shop with JSON. The body is one of: `page_urls`, `page_uniques`, `page` plus `sort`, or cursor pagination (`cursor` plus `sort: product_id_desc`). Sort options: `date_added_desc` (required for all shops), `date_updated_desc` (required for large shops and builders), `product_id_desc` (cursor mode). Pages are exactly 100 products except the last.
- Authentication: header `X-Torob-Token` carries a JWT signed with Torob's EdDSA (ed25519) key, plus `X-Torob-Token-Version: 1`. The shop must verify the signature, `exp`, `nbf` and that `aud` equals the shop's own host name, and keep its clock in sync. The public key is printed in the token guide (read it from S1 at build time; it is not copied here).
- Response fields: `page_unique` (stable, max 200 chars), `page_url` (absolute, max 1500), `title` (max 500), `current_price`, `availability` (boolean), `image_links` (first is the main image), `spec` (a dictionary, an empty one if none), `date_added` (ISO 8601 with time zone). Optional: `subtitle`, `product_group_id` (groups variants; variants share it and differ in `page_unique`), `old_price`, `category_name`, `short_desc`, `guarantee` (max 200), `date_updated`, and marketplace-only `seller_name` and `seller_city`.
- **`current_price` is an integer in Toman**, never null; `0` marks an unavailable product, and `availability: true` with price `0` shows as free. Deleted or hidden products are left out of lists and return an empty array for single lookups.
- Images should be at least 900x900 pixels; below 600x600 are not displayed; no thumbnails.
- Errors: `400` with `{"error": "..."}` for bad requests, `401` for a bad token.
- Product Webhook (optional, real time): `POST https://api.torob.com/update/webhook/v1/` with a Bearer token from Torob support, up to 100 items per call, at most 20 requests a minute per shop; it only queues a refresh.

**Terms that constrain the shop (S3).**
- Price and availability of every option must be readable by Torob's robots from the default option, **and available on the product page even with JavaScript disabled**.
- The shown price must be the all-in price. The only extra at checkout may be a normal, trackable shipping fee. Absurd undercutting by tiny amounts is penalized.
- Prices and stock must be kept current. A mismatch gets the shop's products flagged "invalid price or stock" for a week, longer on repeats, and cooperation ends after the third time. A banner asking customers to "check price or stock before buying" counts as stale data.
- Default dispatch assumption: every item in stock, ready and shipped within 3 business days (up to 5 as an exception). Longer lead times must be stated in the product title. Pre-sale is allowed only for shops with at least a year of clean history and approval.
- The shop name on Torob must equal the name shown on the site, and a distinguishing word of the name must appear in the domain. The name may not contain "Torob" and neither may the domain.
- The shop must follow the e-commerce law (S3 links the law text at `codal.ir/Provisions/ECommerceAct.htm`, useful for the tax and law ticket); the law outranks the shop's own rules.
- The shop must answer order follow-ups and complaints for everyone who files one on Torob, and must: tell buyers shipping terms, time and cost before ordering; send the order number and an exact invoice to the buyer's email and mobile number after ordering; let buyers track an order by order number on its website; allow two-way contact; ship on time, ship the right and a working item, and pack it properly.
- A shop may not send buyers to another shop to complete an order (no intermediary or resale of other sites' products).
- Prohibited products include (list can change; re-read it before building feeds): tobacco and alcohol, **ESET antivirus**, drones, hidden cameras and trackers, walkie-talkies, e-cigarettes, certain named phone models, supplements without a licence, layered identity-document files, and others.
- In rare cases (under 1 percent, per S3) Torob may require a secure payment gateway before cooperation continues.

## Emalls

**How a shop gets listed (S4).**
- An online shop needs an Enamad and an active shopping cart. Registration is a form with an SMS code, then the process continues by phone.
- Documents: for most online shops the Enamad and the Enamad owner's national ID card; phones and tablets additionally need a trade licence; home appliances, **digital goods** (consoles, headphones, smartwatches) and computers and peripherals (laptops, graphics cards, hard drives) need one of a trade licence, an official lease or a postal address confirmation; food supplements need a food and drug authority licence.
- Pay-per-click packages are bought in advance, with no time limit; a separate "special plan" buys better placement.
- After the documents are approved and the panel is charged, Emalls' robot finds and imports the shop's products (typically 3 to 5 business days, up to 10) and links them to Emalls product pages.
- Emalls offers a plugin for WordPress shops (S4).

**What Emalls publishes for developers.** I found **no written API or feed specification** for custom shops. The only first-party machine-readable artifact is the official plugin for WordPress with WooCommerce (S5), described below as observed behaviour, not as a specification.

**Observed in the official plugin v1.3.0 (S5, source read, not executed).**
- It exposes `POST /wp-json/emalls_ext/v1/products` with parameters `token`, `limit` (capped at 100), `page` and `variation`.
- Emalls sends a `token`; the plugin validates it by calling an Emalls server (`https://emalls.ir/swservice/wp_plugin.ashx`, sending the token, the shop domain and the plugin version) and caches a valid result for one hour.
- The response has `count`, `max_pages` and `products`, plus metadata (plugin, WordPress, PHP and WooCommerce versions). Each product has: `title`, `subtitle`, `page_unique`, `current_price`, `old_price`, `availability` (the WooCommerce stock status string), `image_links`, `image_link`, `category_name`, `page_url`, `short_desc`, `spec`, `date_added`, `date_updated` (ISO 8601), `product_type`, `registry` (the phone registry value, taken from a specification named "registry"), `guarantee`.
- The price unit is not stated anywhere; the plugin sends WooCommerce's stored price as is.

## Docs needed (unresolved; do not guess)

1. **Emalls:** the written integration specification for custom (non-WooCommerce) shops: endpoint, authentication, fields, price unit, availability values, update frequency; and whether crawling alone is enough. Ask Emalls (a `to-questionnaire` item).
2. **Torob:** confirm that the GitHub repository is the official documentation location; how API tokens are issued; whether a new shop must implement Product API v3 or may rely on crawling; the tariff (`/s/tariff`) and the integration guide (`/s/shopIntegrationGuide`), which were not fetched; the independent-domain rule (unverified).
3. **Both:** current fees and click prices, which were not fetched or not public.

## What this means for later tickets (facts to weigh, not decisions)

- **Money rules.** Torob v3 wants integer Toman. If the platform stores Rials, the feed must convert exactly. Torob also wants one all-in price per option with shipping as the only extra.
- **Catalog.** Variants need a stable unique id and a shared group id; a guarantee string; a specification dictionary; main images of at least 900x900; an absolute URL per variant. Each product may need an "exclude from price-comparison feeds" flag, because the prohibited-product list includes antivirus software and other items a digital and electronics store might sell.
- **Storefront and SEO.** Price and availability must be in the server-rendered HTML, readable without JavaScript, with a default option selected. No "ask before buying" banners.
- **Orders and after-sales.** Order numbers and exact invoices go to the buyer's email and mobile; buyers can track an order by number; dispatch within 3 business days by default; lead times stated on the product; answer complaints.
- **Business accounts.** Both aggregators require an Enamad before listing; Emalls asks for further documents for phones, digital goods and computers. The prices and terms of both are for the owner to check.
- **Feeds.** Torob has a written API to build against; Emalls does not (yet). Product pages must be crawler-friendly either way.
