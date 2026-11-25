# Triage Labels

Five canonical triage roles. The label string is exactly the role name, applied with `bd label add <id> <label>`.

| Label              | Meaning                                  |
| ------------------ | ---------------------------------------- |
| `needs-triage`     | Maintainer needs to evaluate this issue  |
| `needs-info`       | Waiting on reporter for more information |
| `ready-for-agent`  | Fully specified, ready for an AFK agent  |
| `ready-for-human`  | Requires human implementation            |
| `wontfix`          | Will not be actioned                     |

These are the defaults; nothing needs creating up front because beads creates a label on first use. Edit the table only if this repo already uses different vocabulary.
