# Issue tracker: beads (`bd`)

Issues live in this repo's beads database (`.beads/dolt/`), driven by the `bd` CLI.
Issue ids look like `<prefix>-a3f2dd`, where the prefix was chosen at `bd init` time.

There is no GitHub/GitLab tracker and no PR request surface: don't call `gh` or `glab`,
and don't write issue files by hand. Markdown TODO lists are not a tracker either.

`.beads/issues.jsonl` is a passive export, not the source of truth. `bd dolt push` /
`bd dolt pull` move issues between machines over the git remote (`refs/dolt/data`).

## Reading

```bash
bd ready                     # unblocked work waiting to be picked up
bd list --status open        # add --label, --priority, --json
bd show <id>                 # description, notes, dependencies, comments
bd children <id>             # sub-issues of a parent
bd search "<text>"           # text search
bd query "<expr>"            # structured query
```

Statuses: `open`, `in_progress`, `blocked`, `deferred`, `closed`, `pinned`, `hooked`.

## Writing

```bash
bd create "<title>" -t bug|feature|task|epic|chore|spike -p 0-4 -d "<description>"
bd create "<title>" --parent <id>        # child issue
bd create -f plan.md                     # one issue per "##" heading
bd update <id> --claim                   # take work atomically
bd update <id> --status in_progress
bd edit <id>                             # rewrite prose in $EDITOR
bd close <id>                            # complete
bd reopen <id>
bd link <id1> <id2> --type blocks|tracks|related|parent-child|discovered-from
bd label add <id> <label>                # also: bd label remove, bd label list-all
bd comment <id> "<text>"                 # also: bd note <id> "<text>"
```

`bd link bd-123 bd-456` means **bd-456 blocks bd-123** — the second id blocks the first.

## Conventions

- Priority 0 (highest) to 4 (lowest); default 2.
- One issue per unit of work. Claim before starting: stale `in_progress` leases get
  reclaimed by other workers, so heartbeat or unclaim if you stall.
- Record a multi-step plan as child issues under one parent, not one giant issue.
- A spec lives in the issue description; don't duplicate it into a markdown file.
- Commit and push only when the user or an orchestrator asks; this file governs where
  issues live, not when to publish them.
