# TECH_STACK (DRAFT: version evidence only)

**Status: draft, not a decision.** Collected on 2026-10-22 because the owner asked that the stack be the latest and taken from official websites. The stack itself is decided at Gate 2, when this file becomes the approved record. Policy and open choices: `DECISIONS.md` D-014.

## 1. How versions are verified

1. Never from memory. Re-run section 5 at the start of Phase 2 and before every install.
2. Sources, in order: the project's official site or docs when the version is readable there; the npm registry `latest` dist-tag and the official GitHub releases (what `npm install` really fetches). If a docs page is stale, the registry wins: Drizzle's "Latest releases" page ends in February 2025, while npm and GitHub show releases from October 2026.
3. Pre-releases (rc, beta, canary, Node "Current", PostgreSQL release candidates) need the owner's approval, item by item.
4. "Newest" only helps if the parts install and work together, so section 3 records a resolver test.

## 2. Snapshot, 2026-10-22

| Component | Newest release | Use today | Official-site check | Notes |
|---|---|---|---|---|
| Node.js | 26.12.1 (Current); newest LTS 24.22.1 "Krypton"; 22.23.4 "Jod" is also LTS; 20.x is end-of-life | **24.22.1 LTS** | nodejs.org/en/about/previous-releases table and nodejs.org/dist/index.json | The page says production should use Active or Maintenance LTS only. It also says even-numbered releases up to 26 become LTS six months after release, so re-check before Gate 2. The sandbox ships 20.20.2 (end-of-life). |
| PostgreSQL | 18.6 (major 18, supported to 2030-11-14); 19 is at RC1 (2026-10-15) | **18.x** | postgresql.org/support/versioning | 14 reaches end of life on 2026-11-12. The owner's installed major is unknown: ask at Gate 2. |
| Next.js | 16.4.2 (2026-10-15) | 16.4.2 | nextjs.org/blog lists 16.4 as the newest minor | Needs Node >= 20.9. React peer: ^18.2 or ^19. |
| React, React DOM | 19.3.1 (2026-10-14) | 19.3.1 | react.dev/versions: "Latest version: 19.3" | |
| TypeScript | 7.0.2 (2026-08-20) | **6.0.3** | devblogs.microsoft.com/typescript lists 7.1, 7.0, 6.0, 5.9 | 7.0.2 cannot be installed with typescript-eslint (section 3). 6.0.3 is the newest 6.x. |
| ESLint | 10.13.0 (2026-10-16) | **9.39.5** | eslint.org/blog lists v10.13.0 as newest | 10.x is outside the peer ranges of 3 plugins that eslint-config-next 16.4.2 depends on (section 3). |
| typescript-eslint | 8.72.0 | 8.72.0 | npm + GitHub only (no site check) | Peer `typescript >=4.8.4 <6.1.0`; the canary 8.72.1-alpha.7 has the same range. |
| Tailwind CSS, @tailwindcss/postcss | 4.3.3 (2026-07-16) | 4.3.3 | tailwindcss.com/blog lists 4.3 as newest | |
| shadcn (CLI) | 4.22.1 (2026-10-18) | 4.22.1 | ui.shadcn.com/docs/changelog mentions 4.22 | Components are copied into the repo, so the "version" is the CLI plus the registry content at install time. |
| lucide-react | 1.61.0 (2026-10-21) | 1.61.0 | page fetched, no version text on it | npm + GitHub only. |
| Zod | 4.6.7 (2026-10-12) | 4.6.7 | zod.dev shows 4.6 | |
| Drizzle ORM / Kit | 0.45.4 (2026-10-08) / 0.31.11 stable; 1.0.0-rc.5 on the npm `rc` tag | 0.45.4 / 0.31.11 | official "Latest releases" page is stale (section 1) | 1.0 is at release-candidate stage: re-check before Phase 5. |
| Vitest (Vite) | 5.0.5 (2026-10-15) (Vite 8.3.5) | 5.0.5 | vitest.dev/blog lists 5.0 as newest | Needs Node ^22.12, ^24 or >= 26. |
| @testing-library/react | 16.3.3 (2026-08-27) | 16.3.3 | npm + GitHub only (no site check) | Needs @testing-library/dom as a peer. |
| Playwright (@playwright/test) | 1.64.1 (2026-10-14) | 1.64.1 | playwright.dev/docs/release-notes lists 1.64 as newest | Needs Node >= 20. |
| axe-core, @axe-core/playwright | 4.14.0 (2026-10-05), 4.14.0 | same | npm + GitHub only (no site check) | @axe-core/playwright needs Node >= 22.12. |
| pg (node-postgres) | 8.23.1 | decide in Phase 2 | npm only | The driver choice (pg or postgres.js 3.4.9) is a Phase 2 decision. |
| @types/node | 26.6.5 (types for Node 26) | **24.x** | npm only | Match the runtime's major version, not the newest number. |
| npm | 12.2.1; Node 24.22.1 bundles 11.19.1 | 11.19.1 (bundled) | registry `engines` field | npm 12 needs Node ^22.22.2, ^24.15.0 or >= 26. npm versus pnpm is a Phase 2 decision. |

Release dates come from the official GitHub releases. Where a row says "npm + GitHub only", no official site was checked for that item.

## 3. Do the newest versions work together? Resolver test, 2026-10-22

Method: lockfile-only installs (`npm install --package-lock-only --ignore-scripts`) in scratch folders outside the repo, using Node 24.22.1 (downloaded from nodejs.org; its sha256 matched `SHASUMS256.txt`) and its bundled npm 11.19.1. Package set: Next.js, React, React DOM, @types/react, @types/react-dom, @types/node@24, Tailwind, @tailwindcss/postcss, PostCSS, Drizzle ORM and Kit, pg, Zod, lucide-react, Vitest, @vitejs/plugin-react, jsdom, Testing Library (react and dom), Playwright, @axe-core/playwright, typescript-eslint, eslint-config-next, plus the TypeScript and ESLint versions in the table.

| TypeScript | ESLint | Result |
|---|---|---|
| 6.0.3 | 9.39.5 | **Resolved cleanly**: 650 packages, 0 warnings. Included Next 16.4.2, React 19.3.1, Tailwind 4.3.3, Drizzle 0.45.4 and Kit 0.31.11, Zod 4.6.7, Vitest 5.0.5 with Vite 8.3.5, Playwright 1.64.1, lucide-react 1.61.0 |
| 6.0.3 | 10.13.0 | Resolved (641 packages) but with 3 "overriding peer dependency" warnings: eslint-plugin-react 7.37.5, eslint-plugin-import 2.32.0 and eslint-plugin-jsx-a11y 6.10.2 declare peers only up to ESLint 9 |
| 7.0.2 | 9.39.5 | **Refused** (`ERESOLVE`): typescript-eslint 8.72.0 requires `typescript >=4.8.4 <6.1.0` |

Limits. This proves dependency resolution only. Not tested: `next build` with TypeScript 6.0.3, any Vitest or Playwright run, whether the three plugins work under ESLint 10, Windows installs. Under the sandbox's own Node 20.20.2 and npm 10.8.2, every package set that included Vite 8 (Vitest 5 alone, @vitejs/plugin-react alone) crashed inside npm ("Cannot read properties of null (reading 'edgesOut')"), while sets without Vite 8 resolved. That says nothing about the packages; it shows this sandbox's npm is too old for them, which is why tooling needs Node 24.

## 4. Choices for the owner at Gate 2

1. **What "latest" means** when the newest version breaks its neighbours (D-014). Recommended: the newest stable set that works together (first row of section 3), upgraded piece by piece as blockers clear.
2. **Node.js:** 24 LTS (recommended), or 26 once it is LTS. The owner's installed Node version is unknown.
3. **PostgreSQL:** target 18. The owner's installed major is unknown. Changing a local install needs a dump and restore or `pg_upgrade`, so any such change needs the owner's approval.
4. **Drizzle:** stable 0.45/0.31 now; 1.0 only once it is stable and before Phase 5 starts.
5. **TypeScript 7 and ESLint 10:** adopt when typescript-eslint and the Next.js lint plugins declare support (re-check at each phase start).
6. **Package manager:** npm (ships with Node on Windows) or pnpm.

## 5. Re-verify (before Phase 2 and before each install)

```bash
# Package versions: tags, then engines and peers of the newest release
npm view <package> dist-tags --json
npm view <package>@latest version engines peerDependencies --json
# Node.js official index: newest release, then newest LTS
curl -s https://nodejs.org/dist/index.json | jq -r '.[0].version, ([.[] | select(.lts)][0] | "\(.version) \(.lts)")'
# PostgreSQL: read the table at https://www.postgresql.org/support/versioning/

# Node 24 for tooling (the sandbox ships Node 20): download, verify, extract into an unsnapshotted folder
V=v24.22.1; T=node-$V-linux-x64.tar.xz; mkdir -p ~/.cache/node24 && cd ~/.cache/node24 \
  && curl -fsSO https://nodejs.org/dist/$V/$T && curl -fsSO https://nodejs.org/dist/$V/SHASUMS256.txt \
  && grep " $T\$" SHASUMS256.txt | sha256sum -c - && tar -xJf $T --strip-components=1 && rm $T

# Resolver test in a scratch folder outside the repo
mkdir -p ~/.cache/resolve && cd ~/.cache/resolve && echo '{"name":"rc","private":true}' > package.json
PATH=~/.cache/node24/bin:$PATH npm install --package-lock-only --ignore-scripts <packages>
```

Also open the official pages named in section 2 and compare. Record version, source URL and date here.
