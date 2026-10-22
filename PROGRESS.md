# PROGRESS

_Last updated: 2026-10-22, session 3 (Phase 1: all research tickets and *Catalog model* resolved)._

## Where we are

**Phase 1 (discovery + spec) is in progress.** Gate 0 was approved on 2026-10-22 (annotated tag `gate-0`). No application code exists. Everything is on GitHub (`main`, public repository).

The decision map is charted: `.scratch/commerce-platform/map.md` with 19 tickets in `.scratch/commerce-platform/issues/` (5 research, 12 grilling, 2 tasks). Resolved so far (6 of 19): all five research tickets (*Torob and Emalls*, *SMS providers*, *Payment gateways*, *Tax and consumer law*, *Shipping and addresses*; findings in `.scratch/commerce-platform/research/`) and the decision ticket *Catalog model*. The owner answered two scope rounds (digital delivery, payments, shipping, languages, scale, existing data, customer features, team); the answers are in the map's Notes. `GLOSSARY.md` has 25 terms.

## Next smallest task

**Next:** resolve **one** grilling ticket per session with the owner (`grilling` and `domain-modeling`; update `GLOSSARY.md` as terms resolve). Unblocked now: *Digital delivery rules* (recommended first, because it unblocks inventory and orders), *Money rules*, *Checkout flow and customer account*, *Phone-OTP policy*, *Payment gateway shortlist and payment rules*, *Shipping fee table*, *Admin panel roles*, *Performance and hosting targets*. Two owner tasks are also open: *Owner accounts and test access* and the *Questionnaire for the owner's accountant and lawyer*. The owner may choose the order; otherwise the first by number.

Frontier command, run in `.scratch/commerce-platform/issues/`:
`for f in *.md; do st=$(sed -n 's/^Status: //p' "$f"); [ "$st" = open ] || continue; ok=1; for b in $(sed -n 's/^Blocked by: //p' "$f" | tr -d ','); do [ "$b" = none ] && continue; grep -q '^Status: resolved' "$b"-*.md || ok=0; done; [ $ok = 1 ] && echo "$f"; done`
At this checkpoint it prints ten tickets: eight grilling tickets and the two owner tasks.

## Phase status

| Phase | Gate | Status |
|---|---|---|
| 0 Skills + workspace | 0 | **complete; Gate 0 approved 2026-10-22** |
| 1 Discovery + spec | 1 | **in progress** (map charted; 6 of 19 tickets resolved) |
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
2. Worth starting now: if you do not have an Enamad (electronic trust symbol), apply for it. Torob, Emalls and payment gateways all need it, and it takes time.
3. When I produce it: send the accountant and lawyer questionnaire (VAT, e-invoices, digital goods refunds). Other tickets proceed with provisional assumptions meanwhile.

Nothing else is pending. Decided on 2026-10-22: keep the repository public (D-013); tickets stay local markdown (D-006); newest stable set that works together (D-014); the brief digest is confirmed (D-011); scope answers from rounds 1 and 2 (map Notes).

Later approvals already foreseen: Impeccable engine first run and build path (Gate 3); stack versions (Gate 2, `TECH_STACK.md` section 4); any Docker use; any destructive database action.

## Session log (latest first)

- **2026-10-22, session 3, Phase 1.** Cold-start checks passed (454 vendored files, git clean, key works). Resolved the remaining research tickets *Payment gateways*, *Tax and consumer law* and *Shipping and addresses* (findings under `.scratch/commerce-platform/research/`), added the owner task *Questionnaire for the owner's accountant and lawyer*, passed the facts into ten dependent tickets, then resolved the decision ticket *Catalog model* with the owner in three `ask_user` rounds (11 questions) and added 10 glossary terms. Details and commands: `report.md`.
- **2026-10-22, session 2, Phase 1.** Gate 0 approved and recorded (D-006 and D-011 accepted), scanned the tracked files for secrets (none), tagged `gate-0`, pushed and verified on GitHub. Read the Phase 1 skills in full. Ran `wayfinder` in chart-the-map mode: two `grilling` rounds with the owner (8 questions), `GLOSSARY.md` created (15 terms), map and 18 tickets written, D-015 added. Resolved the research tickets *Torob and Emalls* and *SMS providers*. Details and commands: `report.md`.
- **2026-10-22, session 1 continued.** After Gate 0 was presented: deploy key, GitHub access, public repo decision, stack version evidence (`TECH_STACK.md`, D-013, D-014). Report: `git show gate-0:report.md`.
- **2026-10-22, session 1, Phase 0.** Inspected the environment; fetched the six required skill repositories at pinned SHAs; vendored them unmodified with a lock file and sha256 manifest; wrote the skill index, usage map, `AGENTS.md`, `CODING_STANDARDS.md`, `DECISIONS.md`, `docs/agents/`, this file and the report; initialised git.

## Known constraints

- Sandbox is Linux (Node 20.20.2, npm 10.8.2, git 2.47.3, Python 3.13); pnpm, yarn, bun, psql, docker and gh are not installed here. The owner's machine is Windows 11 with Node, PostgreSQL and Docker.
- The sandbox's Node 20.20.2 is end-of-life and too old for Vitest 5 and @axe-core/playwright (Node >= 22.12); its npm 10.8.2 crashes on package sets that include Vite 8. For tooling, download Node 24 LTS into `~/.cache/node24` (sha256-checked; commands in `TECH_STACK.md` section 5). `~/.cache` is not snapshotted, so repeat this each session that needs it.
- No skill loader, no sub-agent tool, no `chrome-devtools` MCP server in this runtime (see `skills/SKILL_USAGE_MAP.md` section 2).
- Git identity and remotes are not persisted between sessions (`.git/config` is excluded from snapshots). Commit with `git -c user.name="Commerce Platform Agent" -c user.email="agent@localhost.invalid" commit ...` and re-add `origin` (checklist step 4).
- `~/.ssh/` (private deploy key, config, known_hosts) lives inside the workspace and is included in snapshots; it is gitignored. Never export or share it; re-apply the `chmod` in checklist step 4 after any restore.
- Research clones and downloads go to `~/.cache/research/` (not snapshotted, not tracked); only findings files under `.scratch/commerce-platform/research/` are kept.

## Resume checklist (cold start)

1. Read `AGENTS.md` -> this file -> `report.md` -> `.scratch/commerce-platform/map.md` -> `skills/SKILL_INDEX.md` (the sections for the skills you will use).
2. Verify the vendored skills: `cd skills/vendor && sha256sum -c CHECKSUMS.sha256 --quiet && echo OK` (PowerShell variant in `skills/SKILL_INDEX.md` section 9).
3. `git status` should be clean at the start of a session; if it is not, find out why before editing.
4. GitHub (D-013): `chmod 700 ~/.ssh && chmod 600 ~/.ssh/config ~/.ssh/eco_shot_deploy_ed25519`; `git remote get-url origin 2>/dev/null || git remote add origin git@github.com:nimazadeh/eco-shot.git`; `ssh -T git@github.com`. A greeting naming `nimazadeh/eco-shot` means the key works; `Permission denied (publickey)` means the owner has not added it (or has removed it).
5. Before every push, all three must hold: `git status --short` prints nothing; `git ls-files .ssh | wc -l` prints 0; and `K=$(sed -n 5p ~/.ssh/eco_shot_deploy_ed25519); if git grep -q -F -e "$K" $(git rev-list --all); then echo LEAK; else echo clean; fi` prints `clean` (never echo `$K`; line 5 of an unencrypted ed25519 key file holds the secret seed, while line 2 is a header shared by every such key and must not be used). Then `git push origin main`; never `--force`. Tag each approved gate `gate-N` (annotated) and push the tag.
6. Do the smallest next task above; checkpoint before about 40 minutes.
