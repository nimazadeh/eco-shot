Labels: wayfinder:map
Effort: commerce-platform

# Map: Iranian digital commerce platform, Phase 1 discovery

## Destination

An owner-approved `PROJECT_SPEC.md` (Gate 1): the one canonical definition of V1 of the single-vendor Iranian store, precise enough that Phase 2 (architecture and API contracts) can start from it, together with `GLOSSARY.md`. Building anything is out of scope for this map.

## Notes

**Domain.** A single-vendor, Persian-first (RTL), mobile-first online store selling digital products and electronics. Customers sign in with phone OTP only; staff too, with extra authorization. Money is integer. Payment and SMS sit behind replaceable adapters. Torob and Emalls feeds come only from their official documentation.

**Every session.** Read `AGENTS.md` and `PROGRESS.md`, then this map (the open tickets are the files in `issues/` that are not resolved). For grilling tickets follow `grilling` and `domain-modeling`: update `GLOSSARY.md` the moment a term is resolved, and offer an ADR only when the decision is hard to reverse, surprising and a real trade-off. For research tickets follow `research` (primary sources only, cite each claim). Use `to-questionnaire` when someone else holds the answer. When the way is clear, `to-spec` writes `PROJECT_SPEC.md`. Tickets that touch authentication, payments, uploads or admin roles also consult Addy `security-and-hardening`.

**Standing preferences.** The owner decides: one `ask_user` round is at most 4 related questions, recommended answer first. I look up facts and never ask the owner for something I can find. Never invent a Torob, Emalls, gateway or SMS format: record "docs needed". No Redis, queues, search engine or state library without demonstrated need. This repository is public: no secrets, personal or customer data, or details of unfixed vulnerabilities in tickets or research files.

**Local adaptations.** One grilling ticket per session. Research tickets are run inline, one after another (there is no sub-agent tool), and their findings are kept in `research/` on `main` instead of a throwaway branch, because the tracker is local markdown and the sources are public. A ticket's `Type:` line is its `wayfinder:<type>` label. Tickets are referred to by title, not by number.

**Scope settled while charting** (owner answers, 2026-10-22; scope facts, not closed tickets):

- Digital products are delivered automatically: digital codes from a code stock, and downloadable files with expiring links.
- Payment is online card payment through a gateway only, behind a replaceable adapter. Refunds are done by staff outside the system at first.
- Physical items ship nationwide with fees from a staff-edited table (province and weight), plus store pickup. Staff enter tracking codes by hand.
- Persian only for the storefront and the admin panel, built so English can be added later.
- Design target: about 100 orders a day, 5,000 products and 1 to 5 staff, without blocking growth.
- Existing data: the owner left the choice to me. Chosen default: start fresh, and staff get a bulk product import and export (CSV) to load the catalog. Reversible in the admin panel ticket.
- Customer features in V1: the core (catalog, search, cart, checkout, orders with status pages, customer account) plus discount codes.
- The team is the owner plus a few staff with different jobs (support, content, warehouse): several roles with separate permissions and an audit log of who changed what.

## Decisions so far

<!-- One line per resolved ticket: [title](issues/NN-slug.md): gist -->

- [SMS providers: which Iranian providers can send OTP, and what do their official docs say?](issues/02-research-sms-providers.md): all four candidates (Kavenegar, SMS.ir, Ghasedak, Melipayamak) document template-based OTP; only SMS.ir documents a sandbox; OTP wording is fixed per approved template; providers limit by IP and rate; prices and the regulator's OTP rule are still unread.
- [Torob and Emalls: what do they officially require, and where are their docs?](issues/03-research-torob-emalls.md): Torob has an official written Product API v3 (JSON, signed JWT, integer Toman) and crawls small shops; Emalls has no public spec for custom shops (robot crawl; WooCommerce plugin only). Both require an Enamad; Torob's terms need price and availability readable without JavaScript.

## Not yet specified

- **Abuse cases and trust boundaries** across OTP, payments, admin and downloads (Addy `security-and-hardening` at spec time). Sharpens once the authentication, payment and admin-roles tickets resolve.
- **Storefront and admin UX briefs** (Impeccable `shape`). Needs the catalog, checkout and admin decisions first; may run in Phase 3.
- **Content and brand inputs the owner must supply**: store name and logo, legal page texts, a plan for product content and photos. Sharpens after the catalog and the tax and law research.
- **SMS message catalog and Persian wording.** After the orders and notifications ticket.
- **V1 release criteria and acceptance checks.** After the tickets above.
- **Hosting provider and deployment shape.** After the performance, availability and hosting ticket.
- **Questionnaire for Torob and Emalls support** (`to-questionnaire`): Emalls' written spec for custom shops, Torob's docs location and token process, fees. Sharpens once the SEO and feeds ticket decides what V1 provides.

## Out of scope

- Wallet or store credit, cash on delivery, and installments: the owner chose gateway-only payment for V1.
- Carrier API integrations and live shipping rates: the owner chose a fee table and manual tracking.
- English storefront or admin panel: Persian only for V1 (built so English can be added later).
- Reviews and Q&A, wishlist, product compare, blog: the owner chose the core plus discount codes.
- Loyalty points and referrals: excluded in every option offered.
- Migrating data from an existing website: the owner left this to me; V1 starts fresh with a CSV import.
- Manual delivery of digital products as the main model: automatic delivery was chosen.
- Multi-vendor marketplace and native mobile apps: the brief asks for a single-vendor, mobile-first web storefront.
