---
name: setup-josjis
description: "Configure this repo for the josjis delivery skills: initialize the beads issue tracker, write the default triage labels, record the Conventional Commits rule, and set the domain doc layout. Run once before first use of /josjis."
disable-model-invocation: true
---

# Setup Josjis

Scaffold the per-repo configuration that the josjis engineering skills assume:

- **Issue tracker**: beads (`bd`). No question — beads is the tracker.
- **Triage labels**: the five canonical roles with their default strings. No question — write them as-is.
- **Commits**: Conventional Commits, always. No question — write the rule.
- **Domain docs**: where `GLOSSARY.md` and ADRs live, and the consumer rules for reading them.

This is a prompt-driven skill, not a deterministic script. Explore, present what you found, confirm once, then write.

## Process

### 1. Explore

Read whatever exists; don't assume:

- `.beads/`: does it exist? Does `bd ready` run cleanly? Is `bd` on `PATH`?
- `git remote -v` and `.git/config`: which host, which repo?
- `AGENTS.md` and `CLAUDE.md` at the repo root: does either exist? Is there already an `## Agent skills` section in either? Is there a managed `BEADS INTEGRATION` block (bd writes and owns it)?
- `GLOSSARY.md` and `GLOSSARY-MAP.md` at the repo root, and the legacy `CONTEXT.md` / `CONTEXT-MAP.md`. If a legacy file exists, offer to `git mv` it to its `GLOSSARY` name and rewrite those names in `docs/agents/domain.md`.
- `docs/agents/`: does this skill's prior output already exist?
- Monorepo signals: a `pnpm-workspace.yaml`, a `workspaces` field in `package.json`, or a populated `packages/*` with its own `src/`. Their absence means single-context, which is almost every repo.

### 2. Present findings, then act

Summarise what's present and what's missing, then take the sections in order. Only the tracker section can ask a question; the rest are recorded without asking.

**Section A: Issue tracker (beads).** State that issues live in this repo's beads database; there is no GitHub/GitLab tracker and no PR request surface. Then make it true:

```bash
bd init --non-interactive   # skip when .beads/ already exists
bd ready                    # must exit 0
```

- If `bd` is missing, stop and tell the user to install it (`docs: https://github.com/gastownhall/beads`) and re-run this skill.
- Pick a sensible issue prefix when `bd init` asks for one; the repo directory name is fine.
- `bd init` adds its own managed `BEADS INTEGRATION` block to `AGENTS.md`. Leave that block alone here — bd owns it and refreshes it on re-init.

Done when `bd ready` prints from inside this directory.

**Section B: Triage labels.** Write the five canonical roles with default strings and no override step: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`. Beads creates a label on first use (`bd label add <id> <label>`), so there is no label-creation step.

**Section C: Commits.** Record Conventional Commits unconditionally. No user choice, no alternative scheme.

**Section D: Domain docs.** Default to **single-context** (one `GLOSSARY.md` + `docs/adr/` at the repo root). Offer **multi-context** (a root `GLOSSARY-MAP.md` pointing to per-context `GLOSSARY.md` files) only when exploration found monorepo signals; then confirm which layout they want.

### 3. Confirm and edit

Show the user a draft of:

- The `## Agent skills` block to add to whichever of `CLAUDE.md` / `AGENTS.md` is being edited (see step 4)
- The contents of `docs/agents/issue-tracker.md`, `docs/agents/triage-labels.md`, `docs/agents/commits.md`, and `docs/agents/domain.md`

Let them edit before writing.

### 4. Write

**Pick the file to edit:**

- If `CLAUDE.md` exists, edit it.
- Else if `AGENTS.md` exists, edit it.
- If neither exists, ask the user which one to create; don't pick for them.

Never create `AGENTS.md` when `CLAUDE.md` already exists (or vice versa); always edit the one that's already there.

If an `## Agent skills` block already exists in the chosen file, update its contents in-place rather than appending a duplicate. Don't overwrite user edits to the surrounding sections.

The block:

```markdown
## Agent skills

### Issue tracker

Issues live in the beads database (`.beads/`), managed with the `bd` CLI. See `docs/agents/issue-tracker.md`.

### Triage labels

Five canonical triage roles, labels named exactly as the roles. See `docs/agents/triage-labels.md`.

### Commits

Conventional Commits for every message: `type(scope): summary`. See `docs/agents/commits.md`.

### Domain docs

[one-line summary of layout: "single-context" or "multi-context"]. See `docs/agents/domain.md`.
```

Then write the docs files using the seed templates in this skill folder as a starting point:

- [issue-tracker-beads.md](./issue-tracker-beads.md): beads issue tracker
- [triage-labels.md](./triage-labels.md): label vocabulary
- [commits.md](./commits.md): Conventional Commits rules
- [domain.md](./domain.md): domain doc consumer rules + layout

### 5. Done

Tell the user the setup is complete and that `/josjis` and the skills it loads now read from these files. Mention they can edit `docs/agents/*.md` directly later; re-running this skill is only necessary to restart from scratch.
