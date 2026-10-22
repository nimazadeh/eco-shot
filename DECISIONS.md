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
- **Status:** proposed (owner confirms at Gate 0).
- **Why:** no git remote exists, the `gh` CLI is not installed here, and the skill recommends local markdown for solo projects without a remote. Defaults: local-markdown tracker under `.scratch/<feature>/`, the five default triage labels, single-context domain docs (`GLOSSARY.md` + `docs/adr/`). `PROJECT_SPEC.md` stays the canonical product spec.
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
- **Status:** proposed (owner confirms the digest at Gate 0).
- **Why:** the master prompt exists only in chat, and sessions must not rely on chat history.
- **Reverse:** delete once `PROJECT_SPEC.md` is approved at Gate 1.

### D-012: Gates are presented as plain text with numbered options
- **Status:** accepted.
- **Why:** the owner's next message is then the explicit approval, and a session ends at the gate. `ask_user` is reserved for material decisions inside a phase.
- **Reverse:** none needed.
