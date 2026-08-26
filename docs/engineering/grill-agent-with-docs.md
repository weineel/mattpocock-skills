## What it does

`grill-agent-with-docs` runs the same autonomous design and implementation flow as [grill-agent](https://aihero.dev/skills-grill-agent), while maintaining the repository's domain model as decisions settle. A Planning [agent](https://www.aihero.dev/ai-coding-dictionary/agent) adopts its own recommendations, and a separate implementation [subagent](https://www.aihero.dev/ai-coding-dictionary/subagent) builds the result.

Only durable domain knowledge is persisted. Resolved vocabulary belongs in `CONTEXT.md`, and a decision becomes an ADR only when it is hard to reverse, surprising without context, and the result of a real trade-off. The Decision log and internal Implementation brief remain transient.

## When to reach for it

You invoke this by typing `/grill-agent-with-docs`, and the agent will not reach for it on its own.

Use it for an autonomous repository change that is likely to sharpen domain language or settle a durable architectural decision.

| What you want | Reach for |
| --- | --- |
| Autonomous decisions without domain-file changes | [grill-agent](https://aihero.dev/skills-grill-agent) |
| Autonomous decisions with domain-model maintenance | `grill-agent-with-docs` |
| A second agent to answer the questions | [grill-agent-team-with-docs](https://aihero.dev/skills-grill-agent-team-with-docs) |
| Human-owned decisions with the same paper trail | [grill-with-docs](https://aihero.dev/skills-grill-with-docs) |

## Prerequisites

Run it in a writable Git repository on the branch you want committed. The [harness](https://www.aihero.dev/ai-coding-dictionary/harness) must support creating and resuming a subagent.

If the root contains `CONTEXT-MAP.md`, it decides which context owns the glossary and ADR. Otherwise the skill uses root `CONTEXT.md` and `docs/adr/`, creating them only when something qualifies.

## The paper trail

The word to watch is **durable**. The skill separates material worth keeping from material needed only for this run:

| Material | Destination |
| --- | --- |
| Resolved domain term | `CONTEXT.md` |
| Hard-to-reverse, surprising trade-off | ADR |
| Question, adopted answer, and reason | Transient Decision log |
| Complex implementation detail | Transient Implementation brief |

This keeps the glossary from turning into a [spec](https://www.aihero.dev/ai-coding-dictionary/spec) and keeps ordinary implementation choices from accumulating as architectural sediment.

## Common questions

**Does `with-docs` save the whole autonomous conversation?**

No. It saves vocabulary and qualifying ADRs. The full Decision log is passed to the implementation agent and summarized to you, but it is not written into the repository.

**Why did no ADR appear?**

Most decisions should not become ADRs. A reversible choice, an obvious choice, or a choice with no real alternative fails the gate. Zero new ADRs can be the correct result.

**Does it publish the Implementation brief as an Issue?**

No. The brief is an internal handoff. Use [to-spec](https://aihero.dev/skills-to-spec) explicitly when the tracker artifact is part of what you want.

**Can two runs update the same glossary at once?**

They should not. Domain files are shared state and need one writer at a time. The agent-run contract also requires product writers to be sequential within one checkout.

## It's working if

- Terms land in `CONTEXT.md` when they settle, not as a dump after implementation.
- The glossary contains vocabulary rather than requirements or code details.
- ADRs are rare and explain a genuine trade-off.
- The Decision log and Implementation brief are absent from the repository.
- A separate implementation subagent produces the task-only commit.

## Where it fits

`grill-agent-with-docs` is the [stateful](https://www.aihero.dev/ai-coding-dictionary/stateful) form of [grill-agent](https://aihero.dev/skills-grill-agent). Use [grill-agent-team-with-docs](https://aihero.dev/skills-grill-agent-team-with-docs) when an independent respondent should answer the design questions, or [grill-with-docs](https://aihero.dev/skills-grill-with-docs) when the decisions remain yours. [ask-matt](https://aihero.dev/skills-ask-matt) routes among them.
