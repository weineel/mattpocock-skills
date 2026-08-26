---
name: grilling
description: Grill a user or delegated agent through a decision tree. Use when the user wants to stress-test an idea, uses a grill trigger, or another skill needs Interactive, Self-directed, or Respondent-agent mode.
---

# Grilling

Map the subject as a **design tree**: every decision branches into the decisions that hang off it. Work relentlessly until every branch has been visited and nothing remains silently assumed.

## Select the mode

Use **Interactive mode** unless the invoking skill explicitly selects another mode.

- **Interactive**: interview the user. Decisions belong to the user.
- **Self-directed**: ask each decision question yourself, recommend an answer, and adopt it. This mode is available only through a skill that explicitly delegates decision authority.
- **Respondent-agent**: interview one persistent respondent subagent. This mode is available only through a skill that explicitly delegates decision authority.

For Self-directed or Respondent-agent mode, read [AGENT-RUN.md](./AGENT-RUN.md) before asking any design question, then follow it through its completion criterion.

## Interactive mode

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round: number each question and give your recommended answer. Then wait for the user's answers before the next round.

Format a round like so:

```
❓ **Q1** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>

---

❓ **Q2** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>
```

Each round the user answers reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a subagent to find it; don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the subagent to report; ask the rest of the frontier now. A fact-finding result may update the tree, but it never counts as the user's answer and never advances a round that is still waiting for the user. The _decisions_ are the user's: put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.
