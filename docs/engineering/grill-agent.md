## What it does

`grill-agent` takes a repository change from an unsettled idea to a validated local commit without waiting for ordinary product decisions. A Planning [agent](https://www.aihero.dev/ai-coding-dictionary/agent) walks the full design tree, adopts its own recommended answer to every question, and records the result in a Decision log.

The Planning agent never writes product code. A separate implementation [subagent](https://www.aihero.dev/ai-coding-dictionary/subagent) receives only the settled decisions, implements them, validates the change, reviews it, and commits it.

## When to reach for it

You invoke this by typing `/grill-agent`, and the agent will not reach for it on its own.

Reach for it when you want the agent to make the ordinary product choices and deliver the change, but you still want planning and implementation isolated. Its siblings split on two axes:

| What you want | Reach for |
| --- | --- |
| Make the decisions yourself | [grill-with-docs](https://aihero.dev/skills-grill-with-docs) |
| Let one agent decide, with no domain files | `grill-agent` |
| Let one agent decide and maintain domain files | [grill-agent-with-docs](https://aihero.dev/skills-grill-agent-with-docs) |
| Have a second agent answer the design questions | [grill-agent-team](https://aihero.dev/skills-grill-agent-team) |

## Prerequisites

Run it in a Git repository on the branch you want committed. The [harness](https://www.aihero.dev/ai-coding-dictionary/harness) must support creating and resuming a subagent. The skill stops before product edits when that capability is absent.

## The Decision log

Every design question gets an adopted answer and a short reason. That Decision log is the contract between planning and implementation, but it stays in the agent [context](https://www.aihero.dev/ai-coding-dictionary/context) rather than becoming a repository file.

The Planning agent chooses the handoff shape by cognitive complexity:

| Settled work | Handoff |
| --- | --- |
| Clear acceptance criteria, existing seams, no new cross-module contract, one focused implementation context | Decision log and acceptance criteria directly |
| New API, schema, migration, cross-module contract, hard-to-reverse choice, or interacting constraints | An internal Implementation brief plus the Decision log |
| Uncertain boundary | Implementation brief |

The internal brief uses a [spec](https://www.aihero.dev/ai-coding-dictionary/spec)-shaped structure, but it is not published or stored.

## Common questions

**Will it ask me anything?**

Only for a real blocker: missing authority, conflicting explicit requirements, an undiscoverable fact that materially changes the outcome, or an irreversible or repository-external action. Ordinary uncertainty becomes a recorded assumption and the run continues.

**Does the complex route invoke `/to-spec`?**

No. `/to-spec` is user-invoked, confirms seams with the user, and publishes an Issue. This skill creates a transient Implementation brief so the autonomous run can continue without changing the Issue tracker.

**Does this make ordinary `/grilling` answer its own questions?**

No. Self-directed mode is selected explicitly by this wrapper. Interactive grilling still waits for the user and retains its final confirmation gate.

**What happens to changes already in my working tree?**

They remain user-owned. The run records the starting state and produces a task-only commit. It pauses when overlapping edits make that separation unsafe.

**Will it deploy the result?**

No. The invocation authorizes local edits, validation, review, and commit. Publishing Issues, deployment, messages, and remote service changes require separate authority.

## It's working if

- The Decision log shows an adopted answer and reason for every material branch.
- Product code is written by a separate implementation subagent.
- Complex work receives an Implementation brief, while simple work avoids one.
- Existing working-tree changes remain outside the task commit.
- The final response names the route, commit, and validation evidence.

## Where it fits

`grill-agent` is an end-to-end alternative to the human-steered build chain. Its closest neighbours are [grill-agent-with-docs](https://aihero.dev/skills-grill-agent-with-docs), which maintains the domain model, and [grill-agent-team](https://aihero.dev/skills-grill-agent-team), which adds an independent respondent. [ask-matt](https://aihero.dev/skills-ask-matt) routes among the full set.
