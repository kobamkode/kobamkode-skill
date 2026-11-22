---
name: josjis
description: Run the full delivery chain from one command — grill the idea with docs, publish a spec, break it into tickets, implement them on an integration branch, then write the PR. Use when the user invokes /josjis or asks for the end-to-end grill-to-PR workflow.
disable-model-invocation: true
---

# Josjis

Run these phases in order. Do not skip a phase, batch phases, or start a later phase before the current one finishes.

Any text after the command is the idea or topic for phase 1.

## Before phase 2

The spec, ticket, and implementation phases expect the project issue tracker and triage vocabulary to be configured. If they are not, tell the user to run `/setup-matt-pocock-skills` and stop.

## When the tracker is Beads

Beads projects add `bv` as a read-only triage engine. It is the dependency and priority oracle; the tracker's triage labels stay the authority on what may be worked. Every `bv` invocation used to select, order, or plan work passes the gate label:

```bash
bv --robot-next --label ready-for-agent
bv --robot-plan --label ready-for-agent --recipe actionable
```

- Never run bare `bv` — it opens a blocking TUI. Use `--robot-*` only.
- A bv pick is a suggestion inside the gate, never an override. A bead without `ready-for-agent` is not work, however high its score.
- Label tickets `ready-for-agent` in phase 3 once they are agent-ready, or bv cannot see them and phase 4 has no queue.
- Claim with the `claim_command` bv emits; transition and close through the tracker as usual.

## Phase 1 — Grill with docs

Load and follow the `grill-with-docs` skill (`~/.agents/skills/grill-with-docs/SKILL.md`).

This phase owns the design interview plus the `GLOSSARY.md` and ADR updates. Its completion condition is the user explicitly confirming a shared understanding. Stop and wait for that confirmation; publish nothing yet.

Once the user confirms, ask one question: **should we prototype the UI before writing the spec?**

- **No** — phase 1 is complete. Move on.
- **Yes** — load and follow the `prototype` skill (`~/.agents/skills/prototype/SKILL.md`), taking its UI branch, while the design is still fresh. Alongside the variants it builds:
  - Give the user a **flow diagram** of the screens and transitions the design implies, in the conversation. Use Mermaid when the medium renders it, otherwise ASCII. The diagram must be reviewable without running the prototype.
  - If the project has no runnable UI stack to host variants, the flow diagram plus sketched screens is the deliverable — say so instead of forcing an unbuildable prototype.
  - Fold the prototype verdict (chosen variant, flow corrections) into the design, glossary, and ADRs before closing the phase.

Phase 1 still closes only when the user explicitly confirms the shared understanding — which now includes the prototype verdict when one was built.

## Phase 2 — Publish the spec

Load and follow the `to-spec` skill (`~/.agents/skills/to-spec/SKILL.md`), using the conversation and docs from phase 1.

Satisfy its seam check with the user first, then publish the spec to the configured tracker with the `ready-for-agent` label. Keep the published spec's issue number, URL, or path as the **spec pointer** for later phases.

## Phase 3 — Publish the tickets

Load and follow the `to-tickets` skill (`~/.agents/skills/to-tickets/SKILL.md`), passing the **spec pointer**.

Satisfy its ticket-breakdown quiz before publishing. Publish one ticket per slice with its blocking edges. Keep the published tickets as the **ticket pointers**.

## Phase 4 — Implement the spec

Load and follow the `implement-spec` skill (`~/.agents/skills/implement-spec/SKILL.md`), passing the **spec pointer** and the **ticket pointers**.

Do not re-interview the user about the design; phases 1–3 settled it. If implementation reveals a genuine contradiction in the spec, surface it and stop rather than silently redefining the work.

Completion condition: the integration branch implements the whole spec, every ticket is resolved the way the tracker closes work (or the draft PR is ready for review), and the code review `implement-spec` runs is clean.

## Phase 5 — Write the PR

Load and follow the `pr` skill (`~/.agents/skills/pr/SKILL.md`).

Build the PR body from the integration branch, spec, tickets, and the evidence produced in phase 4. Apply it to the PR opened in phase 4. If no PR exists, open one when the tracker closes work through PRs; otherwise present the body.

Stop after reporting the PR and the integration branch.
