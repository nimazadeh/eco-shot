# PROGRESS

_Last updated: 2026-10-22, session 1 (Phase 0)._

## Where we are

**Phase 0 (skills + workspace) is complete and waiting for the owner's Gate 0 approval.** No application code exists. Phase 1 has not started and must not start before approval.

## Next smallest task

- **If the owner approves Gate 0:** start Phase 1 in a fresh session. Read `AGENTS.md`, this file, `report.md`, `docs/owner-brief.md` and `skills/SKILL_INDEX.md`; then open `skills/SKILL_USAGE_MAP.md` section 5 (Phase 1) and run Matt Pocock's `wayfinder` "chart the map" mode: name the destination (an approved `PROJECT_SPEC.md`), grill breadth-first with `ask_user`, and write `.scratch/commerce-platform/map.md` with the first decision tickets. Seed topics from the brief (not decisions): payment gateway adapter(s), SMS provider(s), shipping options, digital-product delivery, inventory model, Rial/Toman rule, RBAC roles, CMS scope, Torob and Emalls feed requirements (official docs needed), OTP policy.
- **If the owner asks for changes:** apply them to the Phase 0 files, re-run the integrity checks below, replace `report.md`, and present Gate 0 again.

## Phase status

| Phase | Gate | Status |
|---|---|---|
| 0 Skills + workspace | 0 | **complete, awaiting approval** |
| 1 Discovery + spec | 1 | not started |
| 2 Architecture + API contracts | 2 | not started |
| 3 Design system + UX prototype | 3 | not started |
| 4 Frontend with mock adapters | 4 | not started |
| 5 DB + backend foundation | 5 | not started |
| 6 Commerce workflows | 6 | not started |
| 7 CMS + integrations | 7 | not started |
| 8 Integration + production audit | 8 | not started |
| 9 Release + handoff | (final) | not started |

## Open items for the owner (answer at Gate 0; none blocks Phase 0)

1. Confirm or change the issue-tracker defaults in `docs/agents/` (local markdown, default labels, single-context domain docs): `DECISIONS.md` D-006.
2. Confirm the digest in `docs/owner-brief.md`, or paste the original master prompt over it: D-011.
3. Decide whether the partial reads listed in `report.md` section 5 must be closed before Phase 1, or at each skill's first use (the default).

Later approvals already foreseen: Impeccable engine first run and build path (Gate 3); stack versions (Gate 2); any Docker use; any destructive database action.

## Session log (latest first)

- **2026-10-22, session 1, Phase 0.** Inspected the environment; fetched the six required skill repositories at pinned SHAs; read their instructions (depth per file in `skills/SKILL_INDEX.md`); vendored the retained paths unmodified with a lock file and sha256 manifest; wrote the skill index, usage map, `AGENTS.md`, `CODING_STANDARDS.md`, `DECISIONS.md`, the setup-skill docs under `docs/agents/`, this file and `report.md`; initialised git. Results and exact commands: `report.md`.

## Known constraints

- Sandbox is Linux (Node 20.20.2, npm 10.8.2, git 2.47.3, Python 3.13); pnpm, yarn, bun, psql, docker and gh are not installed here. The owner's machine is Windows 11 with Node, PostgreSQL and Docker.
- No skill loader, no sub-agent tool, no `chrome-devtools` MCP server in this runtime (see `skills/SKILL_USAGE_MAP.md` section 2).
- Git identity is not persisted between sessions (`.git/config` is excluded from snapshots). Commit with `git -c user.name="Commerce Platform Agent" -c user.email="agent@localhost.invalid" commit ...`.

## Resume checklist (cold start)

1. Read `AGENTS.md` -> this file -> `report.md` -> the specs listed above -> `skills/SKILL_INDEX.md`.
2. Verify the vendored skills: `cd skills/vendor && sha256sum -c CHECKSUMS.sha256 --quiet && echo OK` (PowerShell variant in `skills/SKILL_INDEX.md` section 9).
3. `git status` should be clean at the start of a session; if it is not, find out why before editing.
4. Do the smallest next task above; checkpoint before about 40 minutes.
