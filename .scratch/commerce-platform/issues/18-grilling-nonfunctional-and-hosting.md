# Performance, availability, privacy, accessibility and hosting targets

Type: grilling
Status: open
Blocked by: 01, 02

## Question

1. **Performance budgets** for a mobile-first store at the design target (about 100 orders a day, 5,000 products).
2. **Availability, backup and restore** expectations.
3. **Privacy and retention** of customer data, codes at rest and logs.
4. **Accessibility** target (WCAG 2.2 AA) and the browser and device support matrix.
5. **Hosting and deployment constraints** found in the research (for example IP restrictions on gateways and SMS).
6. **Windows-friendly local install.** The scope of the English-only BAT installer.

## Context

- Use the stricter of the two performance budget sets until measured (`skills/SKILL_USAGE_MAP.md` section 3).
- From the Torob and Emalls research: product price and availability must be readable with JavaScript disabled (Torob's terms).

## Inputs

- Skills: `grilling` and `domain-modeling`. Glossary: `GLOSSARY.md`.

## Done when

Each numbered item has an owner decision (or moves to the fog or out of scope), `GLOSSARY.md` is updated as terms resolve, and the answer gists the decisions for the map.
