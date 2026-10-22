# Report: Phase 0 (skills and workspace)

_Session 1, 2026-10-22 (sandbox clock: clones at 14:32 UTC, checkpoint about 14:55 UTC). This is the only rolling report; each session replaces it._

## 1. Phase and task

Phase 0 only: inspect the workspace and agent capabilities, read the required skill repositories' current instructions, install them correctly, create the skill index, the usage map and the agent instruction file, verify cross-session resumability, and present Gate 0. **No application code was written.**

## 2. Result in brief

- All six required sources were fetched at pinned revisions and vendored **unmodified** under `skills/vendor/` (454 files, lock file, sha256 manifest). Integrity check: **PASS**.
- This runtime cannot activate skills natively and has no sub-agent tool, so activation is pointer-based (read at point of use). That limitation, and every adaptation it forces, is documented rather than hidden.
- Reading depth is stated per source in section 5. **Not everything was read in full**; section 5 lists exactly what was not, and those files are read at first use, or earlier if the owner prefers.
- Nothing is blocked for Phase 1. Three confirmations are requested at Gate 0 (section 10).

## 3. Files created

| Path | Purpose |
|---|---|
| `AGENTS.md` | Thin instruction file: session start, gates, skill pointers, owner guardrails, the setup skill's `## Agent skills` block |
| `PROGRESS.md` | Phase table, next smallest task, open owner items, resume checklist |
| `report.md` | This report |
| `DECISIONS.md` | Phase 0 decisions D-001 to D-012 with reasons and reversal |
| `CODING_STANDARDS.md` | Owner's code rules in one place (read by Matt `code-review`) |
| `skills/SKILL_INDEX.md` | Inventory of 73 skills (72 `SKILL.md` skills plus Impeccable) plus Impeccable's commands and 4 sub-agent roles, with source, local path, mode, purpose, triggers, read depth, project use; install evidence; gaps |
| `skills/SKILL_USAGE_MAP.md` | Activation semantics, environment adaptations, severity mapping, precedence rules, phase plan, task lookup, version tensions, approval triggers |
| `skills/vendor/<owner>-<repo>/` | Six vendored sources; `SOURCES.lock.json` (URL, SHA, commit date, license, retained and excluded paths); `CHECKSUMS.sha256` (454 files) |
| `docs/agents/{issue-tracker,triage-labels,domain}.md` | Output of the setup skill, built from its seed templates (template text verbatim, project mapping appended) |
| `docs/owner-brief.md` | Durable digest of the master prompt (the original exists only in chat) |
| `.gitignore`, `.gitattributes` | Secrets and build output ignored; LF repo text, CRLF for `.bat`/`.cmd`/`.ps1`, `skills/vendor/**` kept byte-exact (`-text`) |

## 4. Commands actually run and what they showed

| Step | What was run | Result |
|---|---|---|
| Environment | Shell inspection of OS, tool versions, skill-related env vars and directories, network reachability | Debian 13; Node 20.20.2, npm 10.8.2, git 2.47.3, Python 3.13, curl, wget, jq. **Absent:** pnpm, yarn, bun, psql, docker, gh. No `.claude/` or `.agents/` directory, no agent env vars, no skill-loader tool. github.com, api.github.com and registry.npmjs.org reachable; raw.githubusercontent.com and codeload.github.com answered 301, so `git clone` was used |
| Fetch | `git clone --depth 1` of the six repositories into a temporary directory | All six exited 0. SHAs below |
| Read | Read-only inspection (`cat`, `sed`, `grep`, `awk`, `find`) of the files listed in section 5; several outputs were truncated by the tool and re-read in smaller pieces | Reads completed as listed; several outputs were truncated by the tool or failed transiently and were repeated in smaller pieces |
| Safety scan | Symlink, Windows reserved-name, illegal-character, case-collision, path-length and CRLF scans over the clones and later over the whole vendored tree and new docs | Symlinks found and skipped (Matt `AGENTS.md`; host-adapter aliases in two Addy repos); 638 paths, longest 112 characters, no name problems, no CRLF in the first three repos. **PASS** |
| Vendor | `python3 vendor.py <ids>` (temporary helper) copying retained paths, preserving file modes, writing the lock and manifest | 454 files copied across six sources |
| Integrity | `sha256sum -c CHECKSUMS.sha256` | **PASS**: 454 of 454 match |
| Consistency | Script comparing lock entries with disk (file counts, retained paths, license files) and `SKILL.md` counts with index rows | **PASS**: 38, 25, 6, 1, 2 `SKILL.md` files match index rows; Impeccable's entry is `skill/SKILL.src.md` |
| Link check | Resolve 299 relative links in vendored markdown | 6 unresolved: 4 point at excluded upstream contributor tooling (`evals/`, `.github/workflows/`), 2 are illustrative placeholders in Matt's docs. No skill's own dependency is dangling |
| Vercel check | Compared each rule title in `rules/` with the compiled guide | 70 of 70 titles present in the compiled `AGENTS.md`; compile step reformats rule text, so the compiled guide (read in full) was used instead of 70 separate files |
| Impeccable check | Diffed `skill/agents/*.md` against upstream's generated `reference/degraded/*.md` | Same text with frontmatter stripped plus a two-line "running inline" preface |
| Generate | `python3 gen_index.py` (temporary) writing `skills/SKILL_INDEX.md` from the vendored frontmatter | Wrote the index; 38 + 25 + 6 + 1 + 2 skill rows plus Impeccable's commands and roles |
| Pointer check | Script resolving every path referenced by `AGENTS.md`, `PROGRESS.md`, `report.md`, `DECISIONS.md`, `CODING_STANDARDS.md`, the index, the usage map and `docs/` | See section 6 |
| Git | `git init`, add, initial commit | See section 6 |

Revisions fetched (full SHAs in `skills/vendor/SOURCES.lock.json`): mattpocock/skills `5beb35b` (2026-10-19, v1.3.1, MIT); addyosmani/agent-skills `aeac41d` (2026-10-13, v0.6.13, MIT); addyosmani/web-quality-skills `afa8da9` (2026-08-24, MIT); anthropics/skills `dbd4588` (2026-10-09, Apache-2.0 for `webapp-testing`); vercel-labs/agent-skills `063bee9` (2026-08-28, MIT declared in README, no LICENSE file); pbakaus/impeccable `8644175` (2026-10-22, v4.7.0, Apache-2.0).

**Not run, on purpose:** any skill script, any upstream installer (`claude plugin install`, `npx skills add`, `gemini skills install`), Matt's `link-skills.sh`, and Impeccable's launcher (it downloads a native binary).

## 5. What was read (exact depth) and what was not

| Source | Read in full | Read in part | Not read in Phase 0 |
|---|---|---|---|
| A. mattpocock/skills | All 38 `SKILL.md` and every supporting file under `skills/` (templates, formats, scripts); README, CLAUDE.md, SCOPE, GLOSSARY, LICENSE, plugin manifests, `.agents/install-block` and `invocation`, ADRs 0001-0002, all 9 `.out-of-scope` notes, both scripts | CHANGELOG (renames, via search), `.agents/writing-docs.md` (first half) | `docs/` pages |
| B. addyosmani/agent-skills | `SKILL.md` of test-driven-development, debugging-and-error-recovery, code-review-and-quality, api-and-interface-design, security-and-hardening (+ its only nested file `references/hardening-patterns.md`), performance-optimization (+ `references/optimization-patterns.md`), browser-testing-with-devtools; README, AGENTS.md, CLAUDE.md; shared references security-checklist, definition-of-done, accessibility-checklist, testing-patterns; personas code-reviewer, security-auditor, test-engineer | | The other 18 skills (inventory from README and frontmatter), `references/{performance,observability}-checklist.md`, `orchestration-patterns.md`, `agents/web-performance-auditor.md`, `docs/` |
| C. web-quality-skills | All 6 `SKILL.md`, `scripts/analyze.sh`, `references/MEASUREMENT.md`, `references/STRUCTURED-DATA.md`, README, AGENTS.md | CLAUDE.md (head) | `references/{A11Y-PATTERNS,WCAG,SECURITY,LCP,INP,CLS,RUM}.md` |
| C. anthropics `webapp-testing` | All files (SKILL.md, `with_server.py`, 3 examples, license) | | Rest of the repository (not vendored) |
| D. vercel-labs | Both `SKILL.md`; the compiled guides for react-best-practices (all 70 rules) and composition-patterns (all 8 rules); README | | The 78 individual rule files under `rules/` |
| E. pbakaus/impeccable | `SKILL.src.md`, launcher script, 14 reference files (routing, shape, craft, craft-floor, adapt, audit, polish, harden, critique, component-review, typeset, mode-persuade, mode-operate, init), finish-reviewer agent, LICENSE, NOTICE | `new-work.md` (about 90%: the middle of the comp-led gate details was truncated), `layout.md` (about 85%) | 25 reference files and 3 agent files (see index section 6), the JS under `scripts/` |

Each unread file must be read in full at first use and logged; the index records this per row.

**Skills applied in Phase 0:** Matt `setup-matt-pocock-skills`, followed manually (explore, defaults, write); its "show the user a draft and let them edit before writing" step is deferred to Gate 0 (D-006). Matt `writing-for-agents`, applied to `AGENTS.md`. No other skill was invoked.

## 6. True pass/fail

| Check | Result |
|---|---|
| Vendored files match manifest | **PASS** (454 of 454) |
| Lock file consistent with disk, every retained path and license file present | **PASS** (6 of 6 sources) |
| Index rows equal `SKILL.md` counts on disk | **PASS** |
| Windows-safe names and paths | **PASS** (638 paths; longest 112 chars) |
| Every path referenced by the project docs exists (later-phase docs whitelisted) | see below |
| Application tests | **None exist; none run.** No code was written |
| Native skill activation | **Not possible** in this runtime (evidence: index section 0) |
| `browser-testing-with-devtools` runnable | **No**: needs a `chrome-devtools` MCP server; no scripts ship with it |
| Impeccable engine runnable | **Not attempted** (owner approval needed) |
| True cold start (a new session resuming from files alone) | **Not yet observable.** The mechanical proxy is the pointer check plus the integrity check; the real test happens when session 2 starts |

- **Pointer check: PASS.** 11 documents, 141 referenced paths, all exist (documents planned for later phases, such as `PROJECT_SPEC.md`, are whitelisted as not yet created).
- **Cold-read proxy: PASS (5 of 5 mechanical checks).** The session-start chain is ordered in `AGENTS.md`; `PROGRESS.md` and `report.md` agree on phase and gate state; the next step is derivable and identical in both (`wayfinder`, Phase 1); all 10 phases are tracked; no application source files and no `.env` files exist.
- **Git:** GIT_RESULT_PLACEHOLDER

## 7. Issues and blockers (exact)

1. No skill loader (runtime limit). 2. No sub-agent tool: Matt `code-review`, `research`, `improve-codebase-architecture`, `implement-spec`; Addy `/ship` fan-out; Impeccable `critique`, finish reviewer, documenter run sequentially with the disclosures their text requires. 3. No `chrome-devtools` MCP; Playwright and Lighthouse CLI are the fallback, unverified here until Phase 4. 4. Impeccable's entry is a template (`SKILL.src.md`) and its verbs need the downloaded engine. 5. Vercel upstream ships no LICENSE file. 6. Reading gaps (section 5). 7. Bash-only helpers need Git Bash or WSL on Windows. 8. Sandbox lacks pnpm, yarn, bun, psql, docker, gh. 9. Long outputs were truncated by the tool several times; each affected file was re-read in smaller pieces.

## 8. Security concerns

- **Third-party instructions are untrusted input.** The agent follows vendored skill text, so a malicious upstream change would be a prompt-injection and supply-chain risk. Mitigation: pinned SHAs, a checksum manifest, no auto-update, owner review on refresh, and `AGENTS.md` owner rules that outrank any skill. Checksums detect local modification, not malice already present at the pinned revision; the pinned text was read as listed in section 5, not audited line by line.
- **Impeccable launcher** downloads and executes a native binary on first run, verified only against a checksum file from the same release origin. Not run; approval gate at Gate 3 (D-009).
- **Scripts that execute or write secrets** (`with_server.py` uses `shell=True` and does not drain server output; `wizard/template.sh` writes `.env` and GitHub secrets; `git-guardrails` hooks): not executed, to be reviewed before use.
- **No credential was seen, requested or recorded.** `.gitignore` excludes `.env*`; the owner's database credential belongs only in an ignored `.env.local`.
- No security or legal compliance is claimed.

## 9. Remaining work

Phase 0: the three owner confirmations below. Then Phases 1 to 9 as in `PROGRESS.md`. Open design tensions to settle at Gate 2 are listed in `skills/SKILL_USAGE_MAP.md` section 7 (shared-store rate limiting without Redis, OTP threat model instead of password guidance, IRR versus Toman in structured data, version-sensitive Vercel rules, WCAG 2.2 target, budgets).

## 10. Exact next action and approval needed

**Gate 0 is presented to the owner. Work stops here until the owner replies.**

1. Approve Gate 0 as is (confirms tracker defaults D-006 and the brief digest D-011); Phase 1 starts in the next session.
2. Approve with changes (name them: for example GitHub or GitLab as the tracker, a different instruction-file name, or edits to `docs/owner-brief.md`).
3. Approve after I close the reading gaps in section 5 first (a follow-up Phase 0 session).
4. Request other changes.

On approval, the next task is `PROGRESS.md` "Next smallest task": Matt `wayfinder`, chart-the-map mode, to define the destination and the first decision tickets for Phase 1.
