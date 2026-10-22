# Catalog model: products, variants and electronics attributes

Type: grilling
Status: resolved
Blocked by: none

## Question

Decide one cluster at a time, in rounds of up to 4 questions:

1. **Product kinds.** Physical product, digital product with codes, digital product with files: can one product mix kinds?
2. **Variants.** Which options exist (color, storage, region, warranty, license duration, ...)? What differs per variant: price, stock, SKU, images, code stock?
3. **Categories, brands and attributes.** Category depth, brand pages, specification tables and filters for electronics.
4. **Product states.** Draft, published, hidden, out of stock, discontinued: what each means to customers and staff.
5. **Price display.** Sale price, original price, price ranges across variants.
6. **Product page content.** Warranty information, delivery type (instant code, file, shipped), specification table, FAQ.
7. **Search.** What customers expect from search in Persian; no external search engine unless proven needed.

## Context

- About 5,000 products; staff get a bulk CSV import and export; Persian only.
- Resolve glossary terms as you go: Product, Variant, Category, Brand, SKU, Warranty.
- From the Torob and Emalls research: variants need a stable unique id and a shared group id, a guarantee (warranty) string, a specification dictionary, and main images of at least 900x900. Consider a per-product "exclude from price-comparison feeds" flag (Torob prohibits some items a digital and electronics store might sell, such as antivirus software).

## Inputs

- Skills: `grilling` and `domain-modeling`. Glossary: `GLOSSARY.md`.

## Done when

Each numbered item has an owner decision (or moves to the fog or out of scope), `GLOSSARY.md` is updated as terms resolve, and the answer gists the decisions for the map.

## Answer

Owner decisions, three `ask_user` rounds on 2026-10-22. The owner took the recommended option on 10 of 11 questions; the other was delegated ("choose best item") and I chose the reversible default.

- **Product kinds.** Two kinds: physical or digital. A digital product delivers a code, a file, or both. One product never mixes physical and digital; an order with both splits into a shipment and a digital delivery, each tracked on its own.
- **Variants.** Each variant has its own SKU, price, stock (or code stock) and images; one product page with a selector.
- **Categories and brands.** A category tree of up to 3 levels with one main category per product; brands are a filter and have brand pages.
- **Specifications.** Fields are defined once per category (they drive filters and comparison tables and feed the Torob and Emalls specification data), plus optional free-form extra rows on a product.
- **Product states.** Draft, Published, Discontinued, Hidden. Out of stock is derived from stock, not a state.
- **Price display.** A price and an optional before price per variant; lists show "from" the lowest variant price; the discount percentage is derived.
- **Product page.** Title, photos, variant selector, price, availability, delivery type (instant code, file or shipped), warranty line, specification table, description, FAQ and automatic related products from the same category. Reviews and Q&A are out of V1.
- **Search.** Text search with Persian normalisation and simple suggestions, run by PostgreSQL; no separate search engine unless measurements show a problem.
- **Warranty.** A warranty line per variant: provider name and duration in months, or none. Shown on the page and passed to Torob and Emalls as the guarantee text.
- **Feed opt-out.** A per-product switch to exclude a product from price-comparison feeds, off by default.
- **Phones and tablets (delegated).** Not special-cased in V1. A registry status is just a specification field of the Mobile category and can be added if phones are sold. Whether phones and tablets will be sold moves to the owner accounts ticket, because Emalls asks for a trade licence for those categories.

Carried from the research (requirements, not decisions): each variant needs a stable unique id and a shared product group id for the feeds; main images should be at least 900x900; absolute URLs; price and availability in server-rendered HTML.

Glossary terms added: Product, Variant, SKU, Category, Brand, Specification, Product state, Before price, plus Warranty and Feed opt-out below. No ADR: none of the decisions met all three criteria (hard to reverse, surprising, a real trade-off).

