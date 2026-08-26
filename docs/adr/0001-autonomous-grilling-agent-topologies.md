# Add explicit autonomous grilling agent topologies

Interactive grilling makes decisions the user's responsibility, but an autonomous build needs explicit authority and role boundaries. We will keep Interactive mode as the default and add Self-directed and Respondent-agent modes selected only by four user-invoked engineering skills. The agent modes produce a transient Decision log, optionally produce an internal Implementation brief, and finish with a task-only commit without invoking `/to-spec` or `/implement`.

## Considered options

- Change `grilling` to answer its own questions by default. Rejected because existing callers rely on human decisions and a final confirmation gate.
- Duplicate the grilling and implementation rules in four wrappers. Rejected because the copies would drift.
- Put every topology behind one skill and infer the intended mode. Rejected because the role and cost differences should be visible at invocation.
- Invoke `/to-spec` and `/implement` from the wrappers. Rejected because both are user-invoked skills and cannot be called by another skill.

## Consequences

The four public entries spend cognitive load to keep their behavior explicit. Their shared agent-run contract lives under the model-invoked `grilling` primitive and is loaded only for the two agent modes. Agent-run skills require subagent support and do not degrade to a single agent when that support is missing.
