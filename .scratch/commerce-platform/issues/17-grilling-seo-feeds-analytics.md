# SEO, Torob and Emalls feeds, and analytics

Type: grilling
Status: open
Blocked by: 03, 08

## Question

1. **Persian SEO.** Slugs (Persian or transliterated), titles, canonical URLs, redirects.
2. **Sitemap and structured data.** Product JSON-LD, using the currency rule from the money rules ticket.
3. **Torob and Emalls.** What V1 provides, based on the official docs found.
4. **Analytics.** Which tool and what consent is needed.

## Context

- Consult web-quality `seo` and its `references/STRUCTURED-DATA.md` when resolving this ticket.
- From the Torob and Emalls research: Torob has a written API (Product API v3) and crawls small shops; Emalls has no public spec for custom shops, so it relies on crawling unless Emalls provides one. Price and availability must be in the server-rendered HTML.
- From the catalog model decisions: a per-product feed opt-out exists (off by default); specifications come from per-category fields plus extra rows; each variant has a unique id and a shared product group; the warranty line is the feed's guarantee text; main images should be at least 900x900.

## Inputs

- Skills: `grilling` and `domain-modeling`. Glossary: `GLOSSARY.md`.

## Done when

Each numbered item has an owner decision (or moves to the fog or out of scope), `GLOSSARY.md` is updated as terms resolve, and the answer gists the decisions for the map.
