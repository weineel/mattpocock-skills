## What it does

`grill-agent-team-with-docs` combines agent-to-agent cross-examination with live domain-model maintenance. Agent A questions an independent Agent B, records the settled decisions, updates the repository's vocabulary and qualifying ADRs, then implements simple work or hands complex work to a fresh Agent C.

The persistent artifacts stay narrow. `CONTEXT.md` holds vocabulary, ADRs hold hard-to-reverse and surprising trade-offs, and the Decision log and Implementation brief remain inside the [agent](https://www.aihero.dev/ai-coding-dictionary/agent) handoff.

## When to reach for it

You invoke this by typing `/grill-agent-team-with-docs`, and the agent will not reach for it on its own.

Use it when a repository change benefits from both an independent respondent and a durable domain paper trail.

| What you want | Reach for |
| --- | --- |
| Agent-team cross-examination without domain files | [grill-agent-team](https://aihero.dev/skills-grill-agent-team) |
| One self-directed planning agent with domain files | [grill-agent-with-docs](https://aihero.dev/skills-grill-agent-with-docs) |
| Agent-team cross-examination with domain files | `grill-agent-team-with-docs` |
| Human-owned decisions with domain files | [grill-with-docs](https://aihero.dev/skills-grill-with-docs) |

## Prerequisites

Run it in a writable Git repository. The [harness](https://www.aihero.dev/ai-coding-dictionary/harness) must support a persistent respondent [subagent](https://www.aihero.dev/ai-coding-dictionary/subagent) and a fresh implementation subagent for complex work.

The repository may use root `CONTEXT.md` and `docs/adr/`, or a `CONTEXT-MAP.md` that assigns them to a narrower context. The domain-modeling rules create files only when a term or decision qualifies.

## Two records with different lifetimes

The **Decision log** exists for this run. The **domain model** exists for later runs. Keeping their lifetimes separate prevents implementation detail from settling into the repository as permanent truth.

| Material | Lifetime |
| --- | --- |
| Question, adopted answer, disagreement, reason | This run only |
| Complex Implementation brief | This run only |
| Canonical domain term | Persisted in `CONTEXT.md` |
| Qualifying architectural decision | Persisted as an ADR |

The simple branch keeps implementation with Agent A and review with Agent B. The complex branch gives the settled Decision log and internal [spec](https://www.aihero.dev/ai-coding-dictionary/spec) to Agent C, while Agent A retains the final audit.

## Common questions

**Does Agent B edit `CONTEXT.md` while answering?**

No. Agent A owns the Decision log and domain-model updates. Agent B is the independent respondent, so it does not write the repository during the grilling phase.

**Does every disagreement become an ADR?**

No. A disagreement is recorded in the transient Decision log. Its resolution becomes an ADR only when the final choice also passes all three ADR gates.

**Can the planning and implementation agents write at the same time?**

No. The workflow has one repository writer at a time. Agent C starts after the question phase and receives settled artifacts.

**What if no vocabulary or ADR qualifies?**

The run can still be successful. `with-docs` applies the domain-modeling discipline; it does not guarantee a file change.

## It's working if

- Agent B answers independently and never writes product or domain files during planning.
- `CONTEXT.md` contains only canonical terms and relationships.
- ADRs are rare, durable decisions rather than a transcript of disagreements.
- The Decision log and Implementation brief do not appear in the repository.
- The final task commit contains the implementation and any qualifying domain updates, but no pre-existing user changes.

## Where it fits

`grill-agent-team-with-docs` is the stateful, cross-examined agent-run flow. Use [grill-agent-team](https://aihero.dev/skills-grill-agent-team) when no domain paper trail is wanted, [grill-agent-with-docs](https://aihero.dev/skills-grill-agent-with-docs) when one planning agent is enough, or [grill-with-docs](https://aihero.dev/skills-grill-with-docs) when the human should make the decisions. [ask-matt](https://aihero.dev/skills-ask-matt) routes among them.
