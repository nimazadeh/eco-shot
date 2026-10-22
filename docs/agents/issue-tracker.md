# Issue tracker: Local Markdown

Issues and specs for this repo live as markdown files in `.scratch/`.

## Conventions

- One feature per directory: `.scratch/<feature-slug>/`
- The spec is `.scratch/<feature-slug>/spec.md`
- Implementation issues are one file per ticket at `.scratch/<feature-slug>/issues/<NN>-<slug>.md`, numbered from `01`, never a single combined tickets file
- Triage state is recorded as a `Status:` line near the top of each issue file (see `triage-labels.md` for the role strings)
- Comments and conversation history append to the bottom of the file under a `## Comments` heading

## When a skill says "publish to the issue tracker"

Create a new file under `.scratch/<feature-slug>/` (creating the directory if needed).

## When a skill says "fetch the relevant ticket"

Read the file at the referenced path. The user will normally pass the path or the issue number directly.

## Wayfinding operations

Used by `/wayfinder`. The **map** is a file with one **child** file per ticket.

- **Map**: `.scratch/<effort>/map.md` (the Notes / Decisions-so-far / Fog body).
- **Child ticket**: `.scratch/<effort>/issues/NN-<slug>.md`, numbered from `01`, with the question in the body. A `Type:` line records the ticket type (`research`/`prototype`/`grilling`/`task`); a `Status:` line records `claimed`/`resolved`.
- **Blocking**: a `Blocked by: NN, NN` line near the top. A ticket is unblocked when every file it lists is `resolved`.
- **Frontier**: scan `.scratch/<effort>/issues/` for files that are open, unblocked, and unclaimed; first by number wins.
- **Claim**: set `Status: claimed` and save before any work.
- **Resolve**: append the answer under an `## Answer` heading, set `Status: resolved`, then append a context pointer (gist + link) to the map's Decisions-so-far in `map.md`.

## Project mapping (this repo; edit freely, per the setup skill)

- Written in Phase 0 with defaults; the owner confirms or switches the tracker at Gate 0. Re-run the setup procedure (`skills/vendor/mattpocock-skills/skills/engineering/setup-matt-pocock-skills/SKILL.md`) only to switch trackers.
- The product-level spec is `PROJECT_SPEC.md` (canonical, approved at Gate 1). "Publish the spec to the issue tracker" for the product-level spec means writing or updating `PROJECT_SPEC.md`. Feature-level specs go to `.scratch/<feature-slug>/spec.md` and reference it instead of restating it.
- Phase-level status lives in `PROGRESS.md`. Tickets are not duplicated there, only linked.
- `.scratch/<feature-slug>/` files are tracked in git. Transient output (HTML reports, drafts) goes to `.scratch/_tmp/`, which is gitignored.
- Applying the `ready-for-agent` label means writing `Status: ready-for-agent` in the ticket file.
