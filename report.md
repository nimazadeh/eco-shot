# Report: Phase 0 (skills and workspace), plus GitHub access and stack-version check

_Session 1, 2026-10-22. Phase 0 work: clones at 14:32 UTC, checkpoint about 14:55 UTC. Follow-up after Gate 0 was presented: about 15:12 to 15:50 UTC. This is the only rolling report; each session replaces it._

> **Update, 2026-10-22 15:44 UTC: Gate 0 was approved by the owner** (reply: "Gate 0 agree go to phase 1 start"; option 1, approve as is, which confirms D-006 and D-011). The deploy key is registered: `ssh -T git@github.com` answered "Hi nimazadeh/eco-shot! You've successfully authenticated". The first push follows this commit, tagged `gate-0`. Phase 1 runs as session 2, which replaces this report.

## 1. Phase and task

Phase 0 only: inspect the workspace and agent capabilities, read the required skill repositories' current instructions, install them correctly, create the skill index, the usage map and the agent instruction file, verify cross-session resumability, and present Gate 0. **No application code was written.**

Follow-up the owner asked for after Gate 0 was presented: (a) work in GitHub, using a key made for this repository only; (b) the stack must be the latest, taken from official websites. Gate 0 was still open while this follow-up was done; the follow-up was housekeeping inside Phase 0 and did not approve it. The owner approved Gate 0 afterwards (see the update above).

## 2. Result in brief

- **Phase 0:** all six sources vendored unmodified under `skills/vendor/` (454 files, lock file, sha256 manifest). Integrity re-checked after this session's edits: **PASS**. Activation is pointer-based because this runtime has no skill loader or sub-agent tool; every adaptation is documented.
- **GitHub:** SSH works from the sandbox. A dedicated ed25519 deploy key exists, GitHub's host keys are pinned and cross-checked, and the owner registered the key (`ssh -T` greets `nimazadeh/eco-shot`). The repository `nimazadeh/eco-shot` was **public and empty**, and the owner chose to keep it public. The first push follows this commit.
- **Versions:** checked on official sites and registries on 2026-10-22 (`TECH_STACK.md`, draft). The newest versions do not all work together: TypeScript 7.0.2 is refused by npm next to typescript-eslint, and ESLint 10.13.0 is outside the peer ranges of three Next.js lint plugins. The newest set that resolved cleanly is in `TECH_STACK.md` section 3, and the owner chose it (D-014). That is a resolver test; nothing was built or run.
- **Correction:** Impeccable `critique.md` was listed as read in full. About 15 lines were never seen. It is now recorded as about 97% (section 5 and `skills/SKILL_INDEX.md`).
- **Reading depth** is stated per source in section 5; not everything was read in full.

## 3. Files created or changed

| Path | Purpose |
|---|---|
| `AGENTS.md` | Instruction file. Changed: Versions rule now points to D-014, new GitHub rule (D-013) |
| `PROGRESS.md` | Phase table, next task, owner items, resume checklist. Changed: GitHub steps, pre-push checks, Node 24 note, open items 1-7 |
| `report.md` | This report (replaced) |
| `DECISIONS.md` | D-001 to D-012 (Phase 0), new D-013 (GitHub deploy key) and D-014 (version policy) |
| `TECH_STACK.md` | **New, draft:** version snapshot, resolver test and Gate 2 choices. Not a stack decision |
| `.gitignore` | Added `.ssh/`, `.npm/`, `.cache/`, `.local/` |
| `skills/SKILL_INDEX.md` | `critique` read depth corrected to about 97% |
| `CODING_STANDARDS.md`, `docs/`, `skills/SKILL_USAGE_MAP.md`, `.gitattributes` | Unchanged since Phase 0 |
| `skills/vendor/**` | Unchanged (454 files, manifest passes) |
| `~/.ssh/` (outside git, ignored) | Private deploy key, public key, `config`, `known_hosts`, nested `.gitignore` |

## 4. Commands actually run and what they showed

### 4a. Phase 0 (earlier in this session)

| Step | What was run | Result |
|---|---|---|
| Environment | Shell inspection of OS, tool versions, skill-related env vars and directories, network reachability | Debian 13; Node 20.20.2, npm 10.8.2, git 2.47.3, Python 3.13, curl, wget, jq. **Absent:** pnpm, yarn, bun, psql, docker, gh. No `.claude/` or `.agents/`, no agent env vars, no skill-loader tool. github.com, api.github.com and registry.npmjs.org reachable; raw.githubusercontent.com and codeload.github.com answered 301, so `git clone` was used |
| Fetch | `git clone --depth 1` of the six repositories into a temporary directory | All six exited 0 |
| Read | Read-only inspection (`cat`, `sed`, `grep`, `awk`, `find`) of the files in section 5; several outputs were truncated by the tool and re-read in smaller pieces | Reads completed as listed in section 5 |
| Safety scan | Symlink, Windows reserved-name, illegal-character, case-collision, path-length and CRLF scans over the clones, later over the vendored tree and new docs | Symlinks found and skipped (Matt `AGENTS.md`; host-adapter aliases in two Addy repos); 638 paths, longest 112 characters, no name problems. **PASS** |
| Vendor | Temporary Python helper copying retained paths, preserving modes, writing the lock and manifest | 454 files across six sources |
| Integrity | `sha256sum -c CHECKSUMS.sha256` | **PASS**: 454 of 454 |
| Consistency | Script comparing lock entries with disk and `SKILL.md` counts with index rows | **PASS**: 38, 25, 6, 1, 2 `SKILL.md` files match index rows |
| Link check | Resolved 299 relative links in vendored markdown | 6 unresolved: 4 point at excluded upstream tooling (`evals/`, `.github/workflows/`), 2 are illustrative placeholders. No skill dependency is dangling |
| Vercel and Impeccable checks | Rule titles vs compiled guide; `skill/agents/*.md` vs upstream degraded files | 70 of 70 titles present; same text with frontmatter stripped plus a two-line preface |
| Git and fresh clone | `git init -b main`, commits `96c9b17`, `5c82420`, `deac671`; fresh clone then `sha256sum -c` | **PASS**: 454 of 454 in the clone |

Revisions (full SHAs in `skills/vendor/SOURCES.lock.json`): mattpocock/skills `5beb35b` (v1.3.1, MIT); addyosmani/agent-skills `aeac41d` (v0.6.13, MIT); addyosmani/web-quality-skills `afa8da9` (MIT); anthropics/skills `dbd4588` (Apache-2.0 for `webapp-testing`); vercel-labs/agent-skills `063bee9` (MIT declared in README, no LICENSE file); pbakaus/impeccable `8644175` (v4.7.0, Apache-2.0).

**Not run, on purpose:** any skill script, any upstream installer, Matt's `link-skills.sh`, Impeccable's launcher.

### 4b. Follow-up after Gate 0

| Step | What was run | Result |
|---|---|---|
| Environment | `id`, `$HOME`, tool lookups, `ssh -V`, `ls ~/.ssh`, `git status`, `git remote -v` | uid 1000 `user`, HOME `/home/user`; OpenSSH 10.0p2; no `~/.ssh`; no git remote (`.git/config` is not persisted) |
| SSH reachability, first try | Bash `/dev/tcp` probe that waited for 80 bytes of banner, to github.com:22 and ssh.github.com:443 | **My probe was wrong.** Both timed out because the banner is shorter than 80 bytes. Not evidence of a block |
| SSH reachability, real test | `ssh -T -o BatchMode=yes` to `git@github.com` and `-p 443 git@ssh.github.com`, throwaway known-hosts file, no key | Both reached `Permission denied (publickey)`: handshake completes on port 22 and on 443 |
| Repo state | `curl https://api.github.com/repos/nimazadeh/eco-shot`; `git ls-remote https://github.com/nimazadeh/eco-shot.git` | `private: false`, default branch `main`, `size: 0`; no refs. Public and empty |
| Key | `ssh-keygen -t ed25519 -N "" -C arena-agent@eco-shot -f ~/.ssh/eco_shot_deploy_ed25519`, then `chmod` | Created. Modes: `.ssh` 700, private key 600, `config` 600. Fingerprint in section 10 |
| Host keys | `curl https://api.github.com/meta` into `known_hosts` for `github.com` and `[ssh.github.com]:443`; `fetch_page` of the docs.github.com fingerprints page; `ssh-keygen -lf known_hosts` | RSA, ECDSA and Ed25519 fingerprints: **3 of 3 match** the docs page |
| Ignore rules | Wrote `~/.ssh/config`, `.ssh/.gitignore` (`*`) and the root `.gitignore` entry; `git check-ignore -v`; `git ls-files .ssh` | 3 of 3 paths ignored; 0 tracked files under `.ssh` |
| Authentication test | `ssh -T git@github.com` via the config alias, pinned host keys, `StrictHostKeyChecking yes` | `Permission denied (publickey)`, exit 255. **Expected: the key is not registered yet.** Pinned host-key verification passed |
| Remote | `git remote add origin git@github.com:nimazadeh/eco-shot.git` | Set for this session only (`.git/config` is lost between sessions) |
| Version data | Temporary Python scripts in `.scratch/_tmp/` (gitignored) querying the npm registry (dist-tags, `engines`, peers), GitHub `releases/latest` and `nodejs.org/dist/index.json`; `fetch_page` of postgresql.org versioning, nodejs.org previous-releases, react.dev/versions and the Drizzle docs "Latest releases" page; HTML fetch of 10 official sites | `TECH_STACK.md` section 2. 8 of 10 sites matched the registry (Next.js, TypeScript, Tailwind, shadcn, Zod to the minor, Vitest, Playwright, ESLint). Lucide's page shows no version text. **Drizzle's docs page is stale** (newest entry February 2025), so npm and GitHub were used. npm `latest` equals the GitHub latest release for all 13 packages compared. Node.js, PostgreSQL and React were read from their official pages |
| Node 24 for tooling | `curl` of `node-v24.22.1-linux-x64.tar.xz` and `SHASUMS256.txt` from nodejs.org, `sha256sum -c`, extract to `~/.cache/node24` | sha256 **OK**; Node 24.22.1 with npm 11.19.1. The folder is not snapshotted |
| Resolver tests | `npm install --dry-run` and `--package-lock-only --ignore-scripts` in scratch folders outside the repo: first npm 10.8.2 on Node 20, then npm 11.19.1 on Node 24 | See `TECH_STACK.md` section 3. TS 6.0.3 + ESLint 9.39.5: **clean** (650 packages). TS 6.0.3 + ESLint 10.13.0: resolves with 3 peer-override warnings. TS 7.0.2: **refused** (`ERESOLVE`). npm 10.8.2 crashed on every set containing Vite 8 (inconclusive; that is why Node 24 was used) |
| Leak checks | `git grep -F` of a key-file line over `git rev-list --all`; `find` plus `grep -lF` over the working tree; positive controls | **My first attempt was wrong twice.** It used key-file line 2, a header shared by every ed25519 key, and a `--` placed before `--exclude-dir` made grep treat the exclusions as file names, so it reported a "match" that was the key file itself inside `.ssh/`. I derived the key format: the secret seed is in line 5. Corrected: history clean, 0 files outside `.ssh/`, both positive controls pass |
| Documented commands re-run | PROGRESS.md checklist steps 4 and 5; the `curl` and `jq` line from `TECH_STACK.md` section 5 | Work as written |
| Integrity after edits | `sha256sum -c CHECKSUMS.sha256` | **PASS**: 454 of 454 |
| Pointer check | Python over `AGENTS.md`, `PROGRESS.md`, `DECISIONS.md`, `TECH_STACK.md` | 56 path-like references; 14 flagged and all reviewed: 6 are URLs, repo names or a fingerprint, 8 are skill-relative names or upstream paths deliberately mentioned as absent. No broken pointer |
| Git | `git commit` | `ccdfae4` (`.ssh/` ignore rule), `4780554` (D-013, D-014, `TECH_STACK.md`, report). The owner-decision edits are in the commit after those; see `git log` |
| Owner decisions | `ask_user` with three questions | Keep the repo public; tickets stay local markdown; newest stable set that works together. Recorded in D-006, D-013, D-014, `AGENTS.md`, `docs/agents/issue-tracker.md`, `TECH_STACK.md`, `PROGRESS.md` |
| Re-test of the key | `ssh -T git@github.com` after the answers | Still `Permission denied (publickey)`, exit 255: the key was not registered yet |
| Key registered | `ssh -T git@github.com` at 15:44 UTC, after the owner added the key | `Hi nimazadeh/eco-shot! You've successfully authenticated, but GitHub does not provide shell access.` (exit 1): registered |
| Pre-push scan | `git grep -l -I -E` for common secret patterns and private-key blocks over all tracked files; list of tracked `.env*` files | No matches in 470 tracked files; 0 `.env` files tracked |

## 5. What was read (exact depth) and what was not

| Source | Read in full | Read in part | Not read in Phase 0 |
|---|---|---|---|
| A. mattpocock/skills | All 38 `SKILL.md` and every supporting document, template and script under `skills/` (the per-skill `agents/openai.yaml` UI metadata only for `ask-matt`); README, CLAUDE.md, SCOPE, GLOSSARY, LICENSE, plugin manifests, `.agents/install-block` and `invocation`, ADRs 0001-0002, all 9 `.out-of-scope` notes, both scripts | CHANGELOG (renames, via search), `.agents/writing-docs.md` (first half) | `docs/` pages |
| B. addyosmani/agent-skills | `SKILL.md` of test-driven-development, debugging-and-error-recovery, code-review-and-quality, api-and-interface-design, security-and-hardening (+ `references/hardening-patterns.md`), performance-optimization (+ `references/optimization-patterns.md`), browser-testing-with-devtools; README, AGENTS.md, CLAUDE.md; shared references security-checklist, definition-of-done, accessibility-checklist, testing-patterns; personas code-reviewer, security-auditor, test-engineer | | The other 18 skills (inventory from README and frontmatter), `references/{performance,observability}-checklist.md`, `orchestration-patterns.md`, `agents/web-performance-auditor.md`, `docs/` |
| C. web-quality-skills | All 6 `SKILL.md`, `scripts/analyze.sh`, `references/MEASUREMENT.md`, `references/STRUCTURED-DATA.md`, README, AGENTS.md | CLAUDE.md (head) | `references/{A11Y-PATTERNS,WCAG,SECURITY,LCP,INP,CLS,RUM}.md` |
| C. anthropics `webapp-testing` | All files (SKILL.md, `with_server.py`, 3 examples, license) | | Rest of the repository (not vendored) |
| D. vercel-labs | Both `SKILL.md`; the compiled guides for react-best-practices (all 70 rules) and composition-patterns (all 8 rules); README | | The 78 individual rule files under `rules/` |
| E. pbakaus/impeccable | `SKILL.src.md`, launcher script, 13 reference files (routing, shape, craft, craft-floor, adapt, audit, polish, harden, component-review, typeset, mode-persuade, mode-operate, init), finish-reviewer agent, LICENSE, NOTICE | **`critique.md` (about 97%: about 15 lines of Assessment B, desktop-app and Codex browser specifics, around lines 218-235, were truncated and never seen)**, `new-work.md` (about 90%: the middle of the comp-led gate details was truncated), `layout.md` (about 85%) | 25 reference files and 3 agent files (see index section 6), the JS under `scripts/` |

Each unread file must be read in full at first use and logged; the index records this per row.

**Skills applied in Phase 0:** Matt `setup-matt-pocock-skills`, followed manually (its "show the user a draft" step is deferred to Gate 0, D-006). Matt `writing-for-agents`, applied to `AGENTS.md`. **No skill was invoked in the follow-up.** Official web pages read in the follow-up are listed in `TECH_STACK.md` section 2.

## 6. True pass/fail

| Check | Result |
|---|---|
| Vendored files match manifest | **PASS** (454 of 454, re-run after this session's edits) |
| Lock file consistent with disk, retained paths and license files present | **PASS** (6 of 6 sources) |
| Index rows equal `SKILL.md` counts on disk | **PASS** |
| Windows-safe names and paths | **PASS** (638 paths; longest 112 chars) |
| Pointers in the edited docs resolve | **PASS** (no broken pointer; 14 flagged items reviewed) |
| SSH reachable from the sandbox, port 22 and 443 | **PASS** (handshake reached authentication on both) |
| GitHub host keys pinned and cross-checked | **PASS** (3 of 3 fingerprints, API vs docs.github.com) |
| `.ssh/` ignored and untracked | **PASS** (3 of 3 paths ignored; 0 tracked) |
| Private key absent from history and from the working tree outside `.ssh/` | **PASS** (corrected check with two positive controls) |
| Deploy-key authentication | **PASS** (15:44 UTC: `ssh -T` greeted `nimazadeh/eco-shot`; write access is proven only by the push itself) |
| Push to GitHub | **Not run at this commit;** the result is recorded in the next report |
| Versions confirmed against official sources | **PASS** with exceptions: 8 of 10 sites matched; Lucide shows no version and Drizzle's page is stale, so those two rely on npm and GitHub |
| The newest versions work together | **NO:** TypeScript 7.0.2 refused; ESLint 10 needs peer overrides. The newest working set resolved cleanly (resolver only) |
| Build, runtime or test execution of any stack package | **Not run.** No code exists |
| Application tests | **None exist; none run** |
| Native skill activation | **Not possible** in this runtime (index section 0) |
| `browser-testing-with-devtools` runnable | **No:** needs a `chrome-devtools` MCP server |
| Impeccable engine runnable | **Not attempted** (owner approval needed) |
| True cold start from files alone | **Not yet observable;** the real test is the next session |

## 7. Issues and blockers (exact)

1. No skill loader (runtime limit). 2. No sub-agent tool: Matt `code-review`, `research`, `improve-codebase-architecture`, `implement-spec`; Addy `/ship` fan-out; Impeccable `critique`, finish reviewer, documenter run sequentially with the disclosures their text requires. 3. No `chrome-devtools` MCP; Playwright and Lighthouse CLI are the fallback, unverified until Phase 4. 4. Impeccable's entry is a template (`SKILL.src.md`) and its verbs need the downloaded engine. 5. Vercel upstream ships no LICENSE file. 6. Reading gaps (section 5). 7. Bash-only helpers need Git Bash or WSL on Windows. 8. Sandbox lacks pnpm, yarn, bun, psql, docker, gh. 9. Long outputs were truncated by the tool several times; affected files were re-read in smaller pieces.
10. Deploy key not registered: **resolved** on 2026-10-22, the key is registered. 11. **The repository is public** (the owner chose to keep it public; see section 8). 12. A deploy key cannot manage Issues or Pull Requests (D-013). 13. `.git/config` is not persisted, so `origin` must be re-added each session (documented). 14. The newest TypeScript and ESLint cannot be adopted yet (D-014). 15. The sandbox's Node 20.20.2 is end-of-life and its npm 10.8.2 crashes on Vite 8 sets; Node 24 needs a per-session download. 16. Two mistakes of mine this session, both caught and corrected before use: the SSH banner probe and the leak-check line and option error (section 4b).

## 8. Security concerns

- **Third-party instructions are untrusted input.** The agent follows vendored skill text, so a malicious upstream change would be a prompt-injection and supply-chain risk. Mitigation: pinned SHAs, a checksum manifest, no auto-update, owner review on refresh, and `AGENTS.md` owner rules that outrank any skill. Checksums detect local modification, not malice already present at the pinned revision; the pinned text was read as listed in section 5, not audited line by line.
- **The private deploy key is stored unencrypted in the sandbox workspace** (`~/.ssh/eco_shot_deploy_ed25519`). A passphrase is impossible for unattended use, and `.ssh/` is not an excluded snapshot path, so it is saved with the workspace. Anyone with access to the workspace or its snapshot can push to the repository until the key is deleted on GitHub. Mitigations: one repository only, revocable in seconds, never printed or committed (ignored twice, verified), and `.ssh/` must not be exported or shared. If in doubt, delete the key on GitHub and I generate a new one.
- **Write access can overwrite history or delete branches.** Rule (D-013): no force-push or deletion without explicit approval. I did not test how GitHub branch protection interacts with deploy keys, so I claim nothing about it.
- **The repository is public** (the owner chose to keep it public). A push publishes everything: today docs and third-party skill copies (licenses preserved; Vercel's upstream has only a README declaration), later specs, source and security notes. Rule (D-013): never push secrets, personal or customer data, or details of unfixed vulnerabilities, so Phase 8 audit findings stay out of the repo until fixed or until the owner explicitly approves. Tracked ticket files are public too.
- **GitHub host keys** were pinned from two independent official sources; a mismatch would have stopped the work.
- **The downloaded Node binary** was checked only against a hash file from the same origin (nodejs.org), without GPG verification. It is used only for resolver tests in a scratch folder and is not shipped.
- **Registry data is trusted as published.** No package code was executed (lockfile-only installs with `--ignore-scripts`).
- **Scripts that execute or write secrets** (`with_server.py` uses `shell=True` and does not drain server output; `wizard/template.sh` writes `.env` and GitHub secrets; `git-guardrails` hooks): not executed, to be reviewed before use.
- **No application credential was seen, requested or recorded.** `.gitignore` excludes `.env*`; the owner's database credential belongs only in an ignored `.env.local`.
- No security or legal compliance is claimed.

## 9. Remaining work

Phase 0 is closed (Gate 0 approved). Then Phases 1 to 9 as in `PROGRESS.md`. For Gate 2: the design tensions in `skills/SKILL_USAGE_MAP.md` section 7 (shared-store rate limiting without Redis, OTP threat model instead of password guidance, IRR versus Toman in structured data, version-sensitive Vercel rules, WCAG 2.2 target, budgets) and the version choices in `TECH_STACK.md` section 4.

## 10. Exact next action and approval needed

**Gate 0 is approved** (owner, 2026-10-22; option 1, approve as is: confirms D-006 and D-011). No approval is pending.

Next: push `main` and the annotated tag `gate-0` (pre-push checks first), then start Phase 1 as session 2: Matt `wayfinder`, chart-the-map mode (`PROGRESS.md` "Next smallest task").

Registered deploy key (public half): `ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIBz7BSMmp4MG76IoY6CN4eJxmxW2wLHk2OhGDFHtuqdg arena-agent@eco-shot`, fingerprint `SHA256:rmTZXuXJcJVnBf8/4iODRGTdqg4vzNdBJIKCiTnhJ98`.
