# Admin panel: roles, permissions, audit log and CMS scope

Type: grilling
Status: open
Blocked by: none

## Question

1. **Roles.** The role list for the owner and a few staff (support, content, warehouse) and the permission matrix.
2. **Audit log.** What is recorded and how long it is kept.
3. **CMS content types.** Homepage blocks and banners, menus, static pages (about, contact, terms, privacy, returns), FAQ.
4. **Staff tools.** Orders list and filters, product management, bulk CSV import and export, code stock loading, discount codes, the shipping fee table, a minimal dashboard and reports.
5. **Customer management.** View and block.
6. **Never allowed.** For example seeing a delivered code again, or deleting the audit log.

## Context

- The team is the owner plus a few staff; several roles with separate permissions and an audit log (owner decision).
- Role grants need the owner's approval when they are implemented (`skills/SKILL_USAGE_MAP.md` section 8).
- From the catalog model decisions: staff manage a three-level category tree, brands, specification fields per category, product states (Draft, Published, Discontinued, Hidden), variants (SKU, price, before price, stock or code stock, images, warranty line) and a per-product feed opt-out; the CSV import and export must cover them.

## Inputs

- Skills: `grilling`, `domain-modeling`, Addy `security-and-hardening`. Glossary: `GLOSSARY.md`.

## Done when

Each numbered item has an owner decision (or moves to the fog or out of scope), `GLOSSARY.md` is updated as terms resolve, and the answer gists the decisions for the map.
