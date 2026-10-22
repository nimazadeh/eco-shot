# Shipping and addresses: carriers, address data and phone formats for Iran

Type: research
Status: resolved
Blocked by: none

## Question

From official sources:

1. Services of Iran Post, Tipax, Chapar and Mahex (confirm the current list) and whether each publishes API docs.
2. The official source of the province and city list.
3. The postal code format.
4. Mobile number formats and how to normalize them (09..., +98..., Persian and Arabic-Indic digits).
5. The national ID format and checksum, only if an official specification exists.

## Context

- The owner chose nationwide shipping from a staff-edited fee table plus store pickup; no carrier integration in V1. This research feeds the table structure and the address form.

## Inputs

- Skill: `research`.
- Write findings to `.scratch/commerce-platform/research/shipping-and-address.md`.

## Done when

The findings file has a citation per claim and a "docs needed" list. The answer lists the facts that the dependent tickets wait on.

## Answer

Findings with a citation and an evidence label per claim: [research/shipping-and-address.md](../research/shipping-and-address.md).

- **Carriers:** only **Tipax** has public API docs with a test platform (token auth, cities, address book, orders, tracking-style order management). Mahex works by contract with a panel and no public API docs were found; Iran Post has address-service APIs but no public shipping-order API docs were found; Chapar has no official page.
- **Postal code:** ten digits (five for the area, one for the building row, four for the place).
- **Provinces:** 31; an official dataset with codes was not obtained.
- **Phone input:** providers accept four spellings (`09...`, `0098...`, `+98...`, `9...`); no official numbering plan was fetched. No official national ID specification was found.
- **Docs needed:** shipping APIs of Iran Post, Mahex and Chapar; an official province and city dataset; Iran Post's postal-code page; the numbering plan and national ID checksum (if needed at all); carrier prices.
