# Agent-run modes

This reference applies only when an invoking skill selects Self-directed or Respondent-agent mode. The invoking skill is the authority for which mode is active and whether domain modeling runs alongside it.

## Preflight

Before changing the product:

1. Confirm the work is in a Git repository. Record the starting `HEAD` and the existing working-tree changes.
2. Confirm the harness can create a subagent and continue the conversation with it. Self-directed mode needs one implementation subagent. Respondent-agent mode needs one persistent respondent and, on its complex branch, a fresh implementation subagent.
3. Treat existing working-tree changes as user-owned. Plan a task-only commit. If overlapping changes make that impossible, pause before editing.

The preflight is complete when the starting state is recorded and the required agent topology is available. If the topology is unavailable, stop before product changes instead of collapsing the roles.

## Authority

Invoking an agent-run wrapper authorizes autonomous decisions, local product changes, validation, review, and task-only commits on the current branch.

Pause for the user only when:

- required authority is missing;
- two explicit requirements conflict;
- a material fact cannot be discovered and different assumptions produce materially different outcomes; or
- the next action has an irreversible or repository-external effect, such as publishing an Issue, deploying, sending a message, or changing a remote service.

For ordinary design choices, reversible trade-offs, and low confidence, choose the strongest recommendation supported by the user request and repository evidence. Record the assumption in the Decision log and continue.

## Build the Decision log

Facts come from the environment. Decisions come from the selected mode.

### Self-directed

Agent A is the Planning agent. For every question on the frontier, record:

```md
### Q<number>: <decision>

**Adopted:** <recommended answer>

**Why:** <brief reason grounded in the request and discovered facts>
```

Adopt the recommendation immediately, recompute the frontier, and continue without waiting for the user.

### Respondent-agent

Agent A is the Driving agent. Create one persistent Agent B as the Respondent agent for the entire grilling phase.

Give Agent B the user's goal, discovered facts, and constraints, but not Agent A's preferred answer. Ask one substantive question at a time. Agent B answers with one recommendation and its reasoning. Agent A verifies facts, challenges weak answers, and records the settled answer in the Decision log before asking the next dependent question.

Agent A normally adopts Agent B's answer. When it conflicts with the user request or repository evidence, state the conflict and continue the exchange. If the disagreement remains, Agent A decides from the user request and verified evidence and records the disagreement.

The Decision log is complete when the frontier is empty, every material assumption is explicit, and each settled question has an adopted answer and reason. Keep it in the agent context. Pass it to the implementing agent and summarize it for the user, but do not write it into the repository.

When a `with-docs` wrapper is active, apply domain modeling while decisions settle. `CONTEXT.md` remains a glossary, and only decisions that pass the domain-modeling ADR gates become ADRs. The Decision log and Implementation brief remain transient.

## Choose the implementation route

Choose **direct handoff** only when all of these are true:

- the goal and acceptance criteria are explicit;
- the work uses existing seams;
- it introduces no new cross-module contract; and
- implementation and validation fit in one focused context.

Choose an **Implementation brief** when the work introduces an API, schema, migration, cross-module contract, hard-to-reverse decision, or enough interacting constraints that a short handoff could lose one. Choose the brief when the boundary is uncertain.

An Implementation brief is an internal handoff, not an Issue. Do not publish it or store it in the repository. Use these sections:

```md
## Problem Statement

## Solution

## User Stories

## Implementation Decisions

## Testing Decisions

## Out of Scope

## Further Notes
```

The brief is complete when an agent with no access to the raw discussion can implement every acceptance criterion without inventing a product decision.

## Implement

Only one agent writes product code at a time.

### Self-directed

Agent A always delegates implementation to one Agent B.

- On the direct route, give Agent B the user request, Decision log, acceptance criteria, starting repository state, and validation requirements.
- On the brief route, also give Agent B the complete Implementation brief.

Agent B implements, validates, reviews the task diff against the handoff and repository standards, and commits only task-owned changes. Agent A then audits the result against the Decision log and acceptance criteria. Send every gap back to the same Agent B until it is resolved.

### Respondent-agent

On the direct route, Agent A implements and validates. The persistent Agent B reviews the task diff against the Decision log and repository standards. Agent A resolves every finding and commits only task-owned changes.

On the brief route, Agent A creates a fresh Agent C with the Decision log, complete Implementation brief, starting repository state, and validation requirements. Agent C implements, validates, reviews, and commits only task-owned changes. Agent A audits the result and sends every gap back to the same Agent C until it is resolved. Agent B remains the Respondent agent and does not review this branch.

## Completion criterion

The agent run is complete only when:

- the Decision log covers the full design tree;
- the selected route's handoff is complete;
- relevant tests, type checks, and repository validations pass, or an allowed blocker is reported;
- all review and audit findings are resolved;
- task changes are committed without absorbing the user's pre-existing changes; and
- the final response reports the adopted decisions, selected route, commit, and validation evidence.
