# Matt Pocock Skills

A collection of agent skills (slash commands and behaviors) loaded by Claude Code. Skills are organized into buckets and consumed by per-repo configuration emitted by `/setup-matt-pocock-skills`.

## Language

**Issue tracker**:
The tool that hosts a repo's issues: GitHub Issues, Linear, a local `.scratch/` markdown convention, or similar. Skills like `to-tickets`, `to-spec`, and `triage` read from and write to it.
_Avoid_: backlog manager, backlog backend, issue host

**Issue**:
A single tracked unit of work inside an **Issue tracker**: a bug, task, spec, or slice produced by `to-tickets`.
_Avoid_: ticket (use only when quoting external systems that call them tickets, or for a **Decision ticket**, see below)

**Decision ticket**:
A `wayfinder` unit: a child **Issue** of a `wayfinder:map` holding a *question* whose resolution is a decision, not a slice of a build to execute. The **decision** qualifier is what keeps it distinct from an implementation ticket; `wayfinder` introduces the term, then uses "ticket".

**Triage role**:
A canonical state-machine label applied to an **Issue** during triage (e.g. `needs-triage`, `ready-for-afk`). Each role maps to a real label string in the **Issue tracker** via `docs/agents/triage-labels.md`.

**Autonomous grilling**:
A grilling process in which the **Planning agent** raises each decision question and immediately adopts its own recommended answer instead of waiting for a human response.

**Planning agent**:
The agent that runs the self-directed form of **Autonomous grilling**, records the resulting decisions, and chooses whether the work needs an **Implementation brief** before handing it off. It does not modify the product.

**Driving agent**:
The agent that questions a **Respondent agent** and owns the resulting **Decision log**. It implements simple work itself and delegates complex work through an **Implementation brief**.

**Respondent agent**:
The persistent agent that answers a **Driving agent** during **Autonomous grilling** and reviews the Driving agent's direct implementation. It does not implement complex work.

**Implementation agent**:
The agent that receives settled work from the **Planning agent** and owns the product changes, tests, review, and commit.

**Implementation brief**:
A self-contained, spec-shaped description of settled work that a **Planning agent** may pass directly to an **Implementation agent**. It is not an **Issue** and is not published to an **Issue tracker**.

**Decision log**:
A transient record of each question raised during **Autonomous grilling**, the recommended answer adopted by the agent, and the reason for that choice. It is handed to the implementing agent and summarized for the user, but is not stored in the project.

## Relationships

- An **Issue tracker** holds many **Issues**
- An **Issue** carries one **Triage role** at a time
- A **Decision ticket** is an **Issue** (a child of a `wayfinder:map`)
- A **Planning agent** hands settled work to an **Implementation agent**
- A **Planning agent** may produce an **Implementation brief** before that handoff
- A **Driving agent** questions one **Respondent agent**
- A **Driving agent** either implements settled work or hands an **Implementation brief** to an **Implementation agent**
- **Autonomous grilling** produces a **Decision log**

## Flagged ambiguities

- "backlog" was previously used to mean both the *tool* hosting issues and the *body of work* inside it. Resolved: the tool is the **Issue tracker**; "backlog" is no longer used as a domain term.
- "backlog backend" / "backlog manager". Resolved: collapsed into **Issue tracker**.
