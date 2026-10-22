# Owner brief (digest of the master prompt)

**Provenance.** The owner's original master prompt exists only in chat. This is a digest of its requirements, reconstructed in the Phase 0 session from the retained task notes, so that later sessions do not depend on chat history. The owner confirmed it by approving Gate 0 on 2026-10-22. `PROJECT_SPEC.md` (Gate 1) supersedes this file as the canonical requirements document; guardrails and working rules live in `../AGENTS.md`, not here.

## Product

A production-grade, **single-vendor** Iranian e-commerce platform selling **digital products and electronics**.

## Required capabilities

- Premium, **RTL/Persian-first, mobile-first** storefront.
- Customer account area.
- Custom admin panel and CMS with **RBAC**.
- Versioned REST API (`/api/v1`) documented with OpenAPI.
- **Phone-OTP-only** authentication for everyone, admins included (admins get extra authorization).
- Catalog: products, variants, pricing, inventory. Orders, payments and shipping.
- **Replaceable adapters** for Iranian payment gateways and SMS providers.
- **Torob and Emalls** product feeds, built only from official format documentation (never invented).
- Evidence for tests, security, performance and accessibility.
- Windows-friendly local installation.

## Stack (versions verified against official docs at implementation time, each with a decision record)

Next.js App Router, React, strict TypeScript, PostgreSQL, Drizzle ORM and Drizzle Kit, Tailwind, shadcn/ui, Lucide, Zod, Vitest and React Testing Library, Playwright, axe, ESLint. Money as integers with explicit Rial/Toman rules. Frontend before backend, using explicit mock adapters with the real DTO shapes.

## Phases and gates

0 skills and workspace -> 1 discovery and spec -> 2 architecture and API contracts -> 3 design system and UX prototype -> 4 frontend with mocks -> 5 DB and backend foundation -> 6 commerce workflows -> 7 CMS and integrations -> 8 full integration and production audit -> 9 release and handoff. Gates 0-8 each need the owner's explicit approval. Sessions run about 30-40 minutes and end at a checkpoint.

## Local environment

Owner machine: Windows 11 with Node.js, PostgreSQL and Docker installed. Host PostgreSQL by default; Docker only with approval. The BAT installer is English-only. The owner's local PostgreSQL credential goes only into an ignored `.env.local` (never hardcoded, committed or logged); `.env.example` holds placeholders.

## Required skill sources (see `../skills/SKILL_INDEX.md`)

mattpocock/skills (complete); addyosmani/agent-skills (test-driven-development, debugging-and-error-recovery, code-review-and-quality, api-and-interface-design, security-and-hardening, performance-optimization, browser-testing-with-devtools); addyosmani/web-quality-skills; anthropics/skills `webapp-testing`; vercel-labs/agent-skills (react-best-practices, composition-patterns); pbakaus/impeccable (`skill/`).

## Do not claim

Full security compliance, legal compliance, or any test, integration or format that was not actually executed or documented by an official source.
