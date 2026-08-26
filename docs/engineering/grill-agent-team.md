## What it does

`grill-agent-team` has one [agent](https://www.aihero.dev/ai-coding-dictionary/agent) cross-examine another before code is written. Agent A asks one decision question at a time, Agent B answers independently, and Agent A verifies the reasoning and records the settled answer in a Decision log.

Agent B sees the user goal and verified facts before answering, but not Agent A's preferred answer. That independent response is the defining constraint. After the grill, Agent A implements simple work and Agent B reviews it; complex work goes through an internal Implementation brief to a fresh Agent C.

## When to reach for it

You invoke this by typing `/grill-agent-team`, and the agent will not reach for it on its own.

Use it when the design benefits from cross-examination before an autonomous implementation.

| What you want | Reach for |
| --- | --- |
| One agent to make every decision itself | [grill-agent](https://aihero.dev/skills-grill-agent) |
| An independent respondent, with no domain files | `grill-agent-team` |
| An independent respondent plus domain-model maintenance | [grill-agent-team-with-docs](https://aihero.dev/skills-grill-agent-team-with-docs) |
| To answer every decision yourself | [grill-with-docs](https://aihero.dev/skills-grill-with-docs) |

## Prerequisites

Run it in a Git repository on the branch you want committed. The [harness](https://www.aihero.dev/ai-coding-dictionary/harness) must support a persistent respondent [subagent](https://www.aihero.dev/ai-coding-dictionary/subagent) and a fresh implementation subagent for complex work. The skill stops before product edits when it cannot preserve those roles.

## Cross-examination and routing

Agent A asks one substantive question at a time. Agent B recommends one answer and explains it. Agent A checks facts and challenges conflicts before moving to the next dependent question. If disagreement remains, Agent A decides from the user request and repository evidence and records the disagreement.

Complexity changes who writes the code:

| Settled work | Writer | Independent check |
| --- | --- | --- |
| Clear, existing seams, one focused context | Agent A | Agent B reviews the task diff |
| New API, schema, migration, cross-module contract, or interacting constraints | Fresh Agent C | Agent A audits against the Decision log and Implementation brief |
| Uncertain boundary | Fresh Agent C | Agent A audits |

Agent C receives settled material, not the raw debate. Product writers never run concurrently in the same checkout.

## Common questions

**Why not just use `grill-agent`?**

Use `grill-agent` when the lower cost of self-directed decisions matters more than a second view. Use this skill when an independent answer could expose assumptions before implementation.

**Does Agent A tell Agent B its preferred answer?**

No. Agent B receives facts and constraints first. Revealing Agent A's preference would turn the respondent into an echo and remove the reason for the extra agent.

**Why does Agent A implement the simple branch?**

The Decision log and code fit one focused [context window](https://www.aihero.dev/ai-coding-dictionary/context-window), so a third handoff would add cost without clearing meaningful complexity. Agent B still reviews the diff before commit.

**Does the complex route invoke `/to-spec`?**

No. Agent A creates a transient, spec-shaped Implementation brief and gives it directly to Agent C. Nothing is published to an Issue tracker.

**Can Agent A batch all the questions?**

No. The persistent Agent B receives one question at a time so each answer can change which branch becomes reachable next.

## It's working if

- Agent B answers without seeing Agent A's preferred answer.
- Questions arrive one at a time and later questions depend on settled answers.
- The Decision log records disagreements instead of smoothing them away.
- Simple work is implemented by Agent A and reviewed by Agent B.
- Complex work is handed to a fresh Agent C with a complete Implementation brief.

## Where it fits

`grill-agent-team` is the cross-examined end-to-end alternative to the normal build chain. Its closest neighbours are [grill-agent](https://aihero.dev/skills-grill-agent), which uses one agent for decisions, and [grill-agent-team-with-docs](https://aihero.dev/skills-grill-agent-team-with-docs), which also maintains the domain model. [ask-matt](https://aihero.dev/skills-ask-matt) routes among them.
