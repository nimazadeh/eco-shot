# PROGRESS

_Last updated: 2026-10-22, session 1 continued (Phase 0, after Gate 0 was presented)._

## Where we are

**Phase 0 (skills + workspace) is complete and waiting for the owner's Gate 0 approval.** No application code exists. Phase 1 has not started and must not start before approval.

After Gate 0 was presented, the owner asked to work in GitHub and for officially verified, latest stack versions. Done: a repo-scoped SSH deploy key was generated and its public half given to the owner; GitHub's SSH reachability and host keys were verified (`DECISIONS.md` D-013); stack versions were checked against official sources and resolver-tested (`TECH_STACK.md`, draft; D-014). The owner decided: keep the repository public, keep tickets as local markdown, use the newest stable set that works together (D-013, D-006, D-014). **Nothing has been pushed yet:** the owner has not registered the key (GitHub still answers `Permission denied (publickey)`).

## Next smallest task

- **Owner adds the deploy key** (GitHub -> Settings -> Deploy keys, tick "Allow write access") and replies "added". Then, in this order: `ssh -T git@github.com` (expect a greeting naming `nimazadeh/eco-shot`); the pre-push checks in the resume checklist; `git push -u origin main` (the remote is empty; never force). Record the result in `report.md`.
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

1. Add the deploy key on GitHub (the public key and its fingerprint are in `report.md` section 10), then reply "added".
2. Confirm the default triage labels and single-context domain docs in `docs/agents/` (D-006; the tracker itself is decided) and the digest in `docs/owner-brief.md`, or paste the original master prompt over it (D-011).
3. Decide whether the partial reads listed in `report.md` section 5 must be closed before Phase 1, or at each skill's first use (the default).
4. Before Gate 2: tell me the Node and PostgreSQL versions installed on the Windows machine (`node -v`, `psql --version`).

Decided on 2026-10-22 (no longer open): keep the repository public (D-013), tickets stay local markdown (D-006), newest stable set that works together (D-014).

Later approvals already foreseen: Impeccable engine first run and build path (Gate 3); stack versions (Gate 2, `TECH_STACK.md` section 4); any Docker use; any destructive database action.

## Session log (latest first)

- **2026-10-22, session 1 continued.** After Gate 0 was presented: generated the deploy key and pinned GitHub's host keys; confirmed SSH reachability; found the repo public and empty; committed the `.ssh/` ignore rule (`ccdfae4`); checked stack versions at official sources and resolver-tested them under Node 24.22.1 (`TECH_STACK.md`, D-014); corrected the read depth of Impeccable `critique.md` to about 97%; replaced `report.md`. The owner then decided: public repo, local-markdown tickets, newest stable set that works together. Re-test: `ssh -T` still `Permission denied (publickey)`, so the key is not registered yet. Nothing pushed.
- **2026-10-22, session 1, Phase 0.** Inspected the environment; fetched the six required skill repositories at pinned SHAs; read their instructions (depth per file in `skills/SKILL_INDEX.md`); vendored the retained paths unmodified with a lock file and sha256 manifest; wrote the skill index, usage map, `AGENTS.md`, `CODING_STANDARDS.md`, `DECISIONS.md`, the setup-skill docs under `docs/agents/`, this file and `report.md`; initialised git. Results and exact commands: `report.md`.

## Known constraints

- Sandbox is Linux (Node 20.20.2, npm 10.8.2, git 2.47.3, Python 3.13); pnpm, yarn, bun, psql, docker and gh are not installed here. The owner's machine is Windows 11 with Node, PostgreSQL and Docker.
- The sandbox's Node 20.20.2 is end-of-life and too old for Vitest 5 and @axe-core/playwright (Node >= 22.12); its npm 10.8.2 crashes on package sets that include Vite 8. For tooling, download Node 24 LTS into `~/.cache/node24` (sha256-checked; commands in `TECH_STACK.md` section 5). `~/.cache` is not snapshotted, so repeat this each session that needs it.
- No skill loader, no sub-agent tool, no `chrome-devtools` MCP server in this runtime (see `skills/SKILL_USAGE_MAP.md` section 2).
- Git identity and remotes are not persisted between sessions (`.git/config` is excluded from snapshots). Commit with `git -c user.name="Commerce Platform Agent" -c user.email="agent@localhost.invalid" commit ...` and re-add `origin` (checklist step 4).
- `~/.ssh/` (private deploy key, config, known_hosts) lives inside the workspace and is included in snapshots; it is gitignored. Never export or share it; re-apply the `chmod` in checklist step 4 after any restore.

## Resume checklist (cold start)

1. Read `AGENTS.md` -> this file -> `report.md` -> the specs listed above -> `skills/SKILL_INDEX.md`.
2. Verify the vendored skills: `cd skills/vendor && sha256sum -c CHECKSUMS.sha256 --quiet && echo OK` (PowerShell variant in `skills/SKILL_INDEX.md` section 9).
3. `git status` should be clean at the start of a session; if it is not, find out why before editing.
4. GitHub (D-013): `chmod 700 ~/.ssh && chmod 600 ~/.ssh/config ~/.ssh/eco_shot_deploy_ed25519`; `git remote get-url origin 2>/dev/null || git remote add origin git@github.com:nimazadeh/eco-shot.git`; `ssh -T git@github.com`. A greeting naming `nimazadeh/eco-shot` means the key works; `Permission denied (publickey)` means the owner has not added it (or has removed it).
5. Before every push, all three must hold: `git status --short` prints nothing; `git ls-files .ssh | wc -l` prints 0; and `K=$(sed -n 5p ~/.ssh/eco_shot_deploy_ed25519); if git grep -q -F -e "$K" $(git rev-list --all); then echo LEAK; else echo clean; fi` prints `clean` (never echo `$K`; line 5 of an unencrypted ed25519 key file holds the secret seed, while line 2 is a header shared by every such key and must not be used). Then `git push origin main`; never `--force`.
6. Do the smallest next task above; checkpoint before about 40 minutes.
