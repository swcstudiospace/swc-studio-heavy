---
name: ultrathink
description: Single Grok slash command for Prompt Uplift plus Graph of Thought and per-node Chain of Thought. Use when Ove says ultrathink, /ultrathink, /uplift, /think, prompt uplift, graph of thought, chain of thought, plan as a DAG, or wants a production-density spec before execution. Do not call prompt-uplift or graph-of-thought; this skill already runs both.
license: MIT
metadata:
  type: workflow
  version: "1.2"
  owner: Ove
  source: swcstudiospace/plugin
  pin: c1742dbed157bbb24279a5c3a45c10dc0fa59663
  public-command: "true"
  absorbs: /ultrathink /uplift /think prompt-uplift graph-of-thought
  companions: retired
  runtime-fetch: "false"
  user-invocable: "true"
---

# Ultrathink

Sole public reasoning command. Same XML contract as `swcstudiospace/plugin` Prompt Uplift + `/think`.

`/ultrathink`, `/uplift`, and `/think` all enter this skill and run Stages 0-4.
Do not load `prompt-uplift` or `graph-of-thought`. Those directories are retired aliases.

Origin pack after merge: https://github.com/swcstudiospace/plugin/tree/main/skill-packs/ultrathink

Load local copies pinned by `references/SOURCE.lock` when present. Never fetch raw.githubusercontent.com/main.
