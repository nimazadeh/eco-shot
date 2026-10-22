# Decisions

The project decision log. Every entry: what was decided, why, and how to reverse it. A decision that is hard to reverse, surprising without context, and a real trade-off also gets an ADR under `docs/adr/` (format: `skills/vendor/mattpocock-skills/skills/engineering/domain-modeling/ADR-FORMAT.md`) linked from here. Status values: `proposed` (needs owner confirmation), `accepted`, `superseded by D-NNN`.

## Phase 0 (2026-10-22)

### D-001: Instruction file is `AGENTS.md`
- **Status:** accepted.
- **Why:** agent-neutral (Codex, Cursor, Copilot, Gemini and others read it), and the brief left the choice to the active agent. No `CLAUDE.md` existed, and Matt Pocock's setup skill says to edit whichever exists and otherwise ask; the brief's delegation answers that.
- **Reverse:** add a one-line `CLAUDE.md` that imports `AGENTS.md` if the owner uses Claude Code. Do not maintain two copies.

### D-002: Skills are vendored in the repo, not installed through upstream installers
- **Status:** accepted.
- **Why:** Arena Agent Mode has no skill loader (no skill tool, no `.claude/skills` or `.agents/skills`), and none of the official routes (`claude plugin install`, `npx skills add -a`, `gemini skills install`, Codex marketplace) targets it. Vendoring gives pinned, reviewable, checksummed copies that survive across sessions and machines. `scripts/link-skills.sh` from Matt's repo was deliberately not run (maintainer script that symlinks into `~/.claude` and `~/.agents` and removes non-symlink targets).
- **Reverse:** the owner may install the upstream plugins on their own machine at any time; `skills/vendor/` stays as the pinned reference.

### D-003: "Activation" means reading the skill at the point of use
- **Status:** accepted.
- **Why:** it is the only mechanism available, and it matches how the skills are designed (progressive disclosure). Claims about a skill are limited to what was read or run and are logged in `report.md`. User-invoked skills run only when the phase plan or the owner calls for them.
- **Reverse:** when the runtime gains a skill loader, retire the adaptation notes in `skills/SKILL_USAGE_MAP.md`.

### D-004: Vendor scope per repository
- **Status:** accepted.
- **Why:** Matt Pocock's whole repo (the brief asks to keep the collection locally). Addy's `agent-skills`: `skills/`, `references/`, `agents/`, slash-command docs and docs, because skills link to the shared references. Everything else is as small as the brief allows: only `webapp-testing` from Anthropic (other skills are unrequested and some are source-available), only the two named Vercel skills, and only Impeccable's `skill/` directory (upstream calls the per-harness copies generated artifacts; the repository is 89 MB). Exclusions and reasons are in `skills/vendor/SOURCES.lock.json`.
- **Reverse:** re-fetch a pinned SHA and add paths; update the lock, checksums and index.

### D-005: Symlinks are skipped, and `skills/vendor/**` is `-text`
- **Status:** accepted.
- **Why:** symlinks break on Windows checkouts, and line-ending conversion would invalidate the sha256 manifest. Only host-adapter aliases and one `AGENTS.md -> CLAUDE.md` alias were skipped; content is unaffected.
- **Reverse:** none needed.

### D-006: Issue tracker defaults (setup skill, run manually)
- **Status:** accepted. The tracker (local markdown) was confirmed by the owner on 2026-10-22 when GitHub Issues was offered as the alternative; the default labels and single-context domain docs were confirmed by the Gate 0 approval the same day.
- **Why:** the `gh` CLI is not installed here, the GitHub remote's deploy key cannot manage Issues (D-013), and the skill recommends local markdown for solo projects. Defaults: local-markdown tracker under `.scratch/<feature>/`, the five default triage labels, single-context domain docs (`GLOSSARY.md` + `docs/adr/`). `PROJECT_SPEC.md` stays the canonical product spec.
- **Reverse:** re-run the setup procedure to switch to GitHub or GitLab; tickets are plain markdown and port easily.

### D-007: Roles that require sub-agents run sequentially in one context
- **Status:** accepted.
- **Why:** no sub-agent tool exists. Each affected skill's own text allows a degraded path and asks for a disclosure (Impeccable `critique` requires a `DEGRADED: single-context` banner). `implement-spec` is not used.
- **Reverse:** use real sub-agents when available.

### D-008: One severity vocabulary for reports
- **Status:** accepted.
- **Why:** three skills use three scales. Mapping is in `skills/SKILL_USAGE_MAP.md`: P0 Critical/Blocking, P1 High/Major/Required, P2 Medium/Minor/Optional, P3 Low/Polish/Nit.
- **Reverse:** none needed.

### D-009: Impeccable's engine binary is not fetched or run without owner approval
- **Status:** accepted.
- **Why:** the launcher downloads a native binary on first run and verifies it only against a checksum file from the same release origin. The skill documents a fallback, so design work does not depend on it. Approval is requested at Gate 3 together with the code-led versus comp-led build path.
- **Reverse:** approve, or set `IMPECCABLE_BIN` to a binary the owner verified.

### D-010: `CODING_STANDARDS.md` is added
- **Status:** accepted.
- **Why:** Matt's `code-review` reads it as its Standards source, and the owner's code rules need a home that is not the always-loaded `AGENTS.md` (his `writing-for-agents` and `retro` skills say to keep `AGENTS.md` to pointers and move enforceable standards elsewhere). Rules are restated from the brief only.
- **Reverse:** merge into another doc; keep one source of truth.

### D-011: `docs/owner-brief.md` makes the brief durable
- **Status:** accepted (the owner confirmed the digest by approving Gate 0 on 2026-10-22).
- **Why:** the master prompt exists only in chat, and sessions must not rely on chat history.
- **Reverse:** delete once `PROJECT_SPEC.md` is approved at Gate 1.

### D-012: Gates are presented as plain text with numbered options
- **Status:** accepted.
- **Why:** the owner's next message is then the explicit approval, and a session ends at the gate. `ask_user` is reserved for material decisions inside a phase.
- **Reverse:** none needed.

## Follow-up after Gate 0 was presented (2026-10-22)

### D-013: GitHub remote reached with a repo-scoped SSH deploy key
- **Status:** accepted. The owner asked to work in GitHub and for a key made for this purpose, and on 2026-10-22 chose to **keep the repository public** (`github.com/nimazadeh/eco-shot`: default branch `main`, no refs at that time). Pushing starts once the owner has registered the key and `ssh -T git@github.com` greets `nimazadeh/eco-shot`.
- **What:** remote `origin` = `git@github.com:nimazadeh/eco-shot.git` (deploy keys work over SSH only). Key: ed25519, no passphrase (it must work unattended), `~/.ssh/eco_shot_deploy_ed25519`, fingerprint `SHA256:rmTZXuXJcJVnBf8/4iODRGTdqg4vzNdBJIKCiTnhJ98`. The owner adds the public half under Settings -> Deploy keys with "Allow write access". GitHub's host keys are pinned in `~/.ssh/known_hosts` (taken from `api.github.com/meta`; all three fingerprints matched `docs.github.com`), with `StrictHostKeyChecking yes`.
- **Handling:** `~/.ssh/` is gitignored (root `.gitignore` plus a nested `*`) and the private key is never printed or committed. `~/.ssh/` sits inside the workspace and the snapshot includes it, so the workspace must not be exported or shared together with `.ssh/`. Before every push: nothing is tracked under `.ssh/`, and no commit contains the private key (commands in `PROGRESS.md`; the search uses key-file line 5, which holds the secret seed).
- **Rules:** push at checkpoints, to `main`. Never force-push, delete a remote branch or tag, or rewrite pushed history without explicit approval. Tag each approved gate `gate-N` (annotated). Because the repository is public, never push secrets, personal or customer data, or details of unfixed vulnerabilities; Phase 8 audit findings stay out of the repo until fixed or until the owner explicitly approves. Tracked ticket files are public too.
- **Limits:** a deploy key does git push and pull only. It cannot use the GitHub API, so it cannot manage Issues, Pull Requests or Actions; the tracker stays local markdown (D-006) unless the owner supplies a separate fine-grained token. `.git/config` is not persisted between sessions, so `origin` is re-added at each session start.
- **Why:** the sandbox cannot use the owner's own credentials; a deploy key is limited to one repository and the owner can revoke it at any time.
- **Reverse:** delete the key in GitHub (Settings -> Deploy keys), delete `~/.ssh/eco_shot_deploy_ed25519*`, run `git remote remove origin`.

### D-014: Versions are the newest stable set that works together, verified at official sources at the time of use
- **Status:** accepted. Owner, 2026-10-22: the stack must be the latest, taken from the official website. Asked how to treat a newest version that breaks its neighbours, the owner chose the newest stable set that works together.
- **Rule:** never take a version from memory. Check official sources before Phase 2 and before each install, and record version, source and date in `TECH_STACK.md`. Pre-releases (rc, beta, canary, Node "Current", PostgreSQL RC) only with owner approval. Runtime: LTS only, as nodejs.org says for production. Database: the newest supported major, never an RC.
- **Evidence (2026-10-22; table and method in `TECH_STACK.md`):** the newest versions do not all work together. TypeScript 7.0.2 is refused by npm next to typescript-eslint 8.72.0 (peer `<6.1.0`, also for its canary). ESLint 10.13.0 is outside the declared peer range of three plugins that `eslint-config-next` 16.4.2 depends on. Node 26 is "Current", not LTS. PostgreSQL 19 and Drizzle 1.0 are release candidates. The newest set that resolved cleanly under Node 24.22.1 and npm 11.19.1 is TypeScript 6.0.3, ESLint 9.39.5, Next.js 16.4.2, React 19.3.1, Tailwind 4.3.3, Drizzle 0.45.4 and Kit 0.31.11, Zod 4.6.7, Vitest 5.0.5 and Playwright 1.64.1. That is a resolver test only: nothing was built or run.
- **Decided:** use that set and upgrade each piece the moment its blocker clears, re-checked at the start of Phase 2 and before each install. Not chosen: pinning the absolute newest numbers (TypeScript 7.0.2 without typescript-eslint, ESLint 10 with overridden peers, Node 26, Drizzle 1.0 RC). The version pins themselves are still approved at Gate 2.
- **Reverse:** change the pins in `TECH_STACK.md` at Gate 2, or later through a new decision.

## Phase 1 (2026-10-22)

### D-015: Phase 1 product decisions live in the wayfinder map and its tickets
- **Status:** accepted.
- **Why:** Matt's `wayfinder` keeps each decision in exactly one place, its ticket, and the map only gists and links it. `DECISIONS.md` therefore records workspace, process and stack decisions, and points to `.scratch/commerce-platform/map.md` for product decisions. `PROJECT_SPEC.md` consolidates them at Gate 1.
- **Local adaptations (also in the map's Notes):** one grilling ticket per session; research tickets run inline one after another (no sub-agent tool), with findings kept in `.scratch/commerce-platform/research/` on `main`; a ticket's `Type:` line is its `wayfinder:<type>` label.
- **Reverse:** none needed.
