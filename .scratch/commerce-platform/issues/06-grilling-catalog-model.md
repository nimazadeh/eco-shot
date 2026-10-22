# Catalog model: products, variants and electronics attributes

Type: grilling
Status: open
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
