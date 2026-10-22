# PROGRESS

_Last updated: 2026-10-22, session 2 (Gate 0 approved; Phase 1 starting)._

## Where we are

**Gate 0 was approved by the owner on 2026-10-22** (reply: "Gate 0 agree go to phase 1 start"; option 1, approve as is, which also confirms D-006 and D-011). Phase 0 is closed. **Phase 1 (discovery + spec) starts now.** No application code exists.

The GitHub deploy key is registered and authenticated (`ssh -T` greeted `nimazadeh/eco-shot` at 15:44 UTC). The first push follows the commit tagged `gate-0`. Decided earlier on 2026-10-22: keep the repository public (D-013), tickets stay local markdown (D-006), newest stable set that works together (D-014).

## Next smallest task

**Phase 1, session 2:** run Matt Pocock's `wayfinder` in "chart the map" mode (`skills/vendor/mattpocock-skills/skills/engineering/wayfinder/SKILL.md`, with `grilling` and `domain-modeling`). Name the destination (an approved `PROJECT_SPEC.md` at Gate 1), grill breadth-first with `ask_user`, and write `.scratch/commerce-platform/map.md` plus the first decision tickets under `.scratch/commerce-platform/issues/`. Seed topics from the brief (not decisions): payment gateway adapter(s), SMS provider(s), shipping options, digital-product delivery, inventory model, Rial/Toman rule, RBAC roles, CMS scope, Torob and Emalls feed requirements (official docs needed), OTP policy. Facts come from primary sources that I look up; decisions come from the owner's answers.

## Phase status

| Phase | Gate | Status |
|---|---|---|
| 0 Skills + workspace | 0 | **complete; Gate 0 approved 2026-10-22** |
| 1 Discovery + spec | 1 | **in progress** (session 2) |
| 2 Architecture + API contracts | 2 | not started |
| 3 Design system + UX prototype | 3 | not started |
| 4 Frontend with mock adapters | 4 | not started |
| 5 DB + backend foundation | 5 | not started |
| 6 Commerce workflows | 6 | not started |
| 7 CMS + integrations | 7 | not started |
| 8 Integration + production audit | 8 | not started |
| 9 Release + handoff | (final) | not started |

## Open items for the owner

1. Before Gate 2: tell me the Node and PostgreSQL versions installed on the Windows machine (`node -v`, `psql --version`).

Decided on 2026-10-22 (no longer open): keep the repository public (D-013); tickets stay local markdown (D-006); newest stable set that works together (D-014); digest in `docs/owner-brief.md` confirmed (D-011); the partial reads listed in `report.md` section 5 stay as the default: each file is read in full at its skill's first use.

Later approvals already foreseen: Impeccable engine first run and build path (Gate 3); stack versions (Gate 2, `TECH_STACK.md` section 4); any Docker use; any destructive database action.

## Session log (latest first)

- **2026-10-22, session 2, Phase 1.** Gate 0 approved. Deploy key authenticated. Recorded the approval (D-006 and D-011 accepted), scanned the tracked files for secrets (none), tagged `gate-0` and pushed. Read the session-start files and the Phase 1 skills in full (`wayfinder`, `grilling`, `domain-modeling` with its two format docs, `grill-with-docs`, `research`, `to-questionnaire`, `to-spec`). Started charting the map.
- **2026-10-22, session 1 continued.** After Gate 0 was presented: generated the deploy key and pinned GitHub's host keys; confirmed SSH reachability; found the repo public and empty; committed the `.ssh/` ignore rule (`ccdfae4`); checked stack versions at official sources and resolver-tested them under Node 24.22.1 (`TECH_STACK.md`, D-014); corrected the read depth of Impeccable `critique.md` to about 97%; replaced `report.md`. The owner decided: public repo, local-markdown tickets, newest stable set that works together.
- **2026-10-22, session 1, Phase 0.** Inspected the environment; fetched the six required skill repositories at pinned SHAs; read their instructions (depth per file in `skills/SKILL_INDEX.md`); vendored the retained paths unmodified with a lock file and sha256 manifest; wrote the skill index, usage map, `AGENTS.md`, `CODING_STANDARDS.md`, `DECISIONS.md`, the setup-skill docs under `docs/agents/`, this file and `report.md`; initialised git.

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
