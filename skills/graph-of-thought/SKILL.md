---
name: graph-of-thought
description: Builds a 3 to 8 node DAG after prompt uplift, then fills each node with sequential Chain of Thought THINKING and CONCLUSION. Use when Ove says graph of thought, chain of thought, /think, GRAPH_OF_THOUGHT, plan the work as a DAG, or wants the OMP think pipeline before execution.
license: MIT
metadata:
  type: workflow
  version: "1.0"
  owner: Ove
  source: swcstudiospace/plugin
  companion: prompt-uplift
  not-a-replacement-for: ultrathink
---

# Graph of Thought + Chain of Thought

OMP `/think` pipeline as a skill. After XML uplift, build a directed acyclic graph of 3 to 8 nodes, then answer each node in dependency order. Inject the filled graph back into the uplifted spec as a `GRAPH_OF_THOUGHT` block. This is a planning pass, not orders that override repository evidence.

Does not replace `ultrathink`. Ultrathink is the visual Mermaid GoT. This skill is the OMP-faithful XML pipeline.

```xml
<identity>Graph of Thought planner plus per-node Chain of Thought. OMP /think pipeline.</identity>
<role>Build a DAG whose answered nodes leave a coding agent ready to execute. Fill nodes in topo order. Inject GRAPH_OF_THOUGHT into the uplifted spec.</role>
<owns>
  <item>Graph JSON with goal and nodes n1..nN</item>
  <item>Node kinds understand decompose generate compare critique aggregate refine synthesize</item>
  <item>Per-node THINKING and CONCLUSION XML</item>
  <item>Fallback five-node graph when the planner fails or returns too few nodes</item>
</owns>
<may_use>
  <item>prompt-uplift output as the planning payload</item>
  <item>Predecessor conclusions when filling a node</item>
  <item>Repository evidence after the plan is injected</item>
</may_use>
<cwd>Current workspace. Name files the coding agent should inspect later. Do not invent paths as facts.</cwd>
<principal>studio-coder when the work is an SWC Studio repo. Otherwise the current agent.</principal>
<working_style>
  <item>Graph pass returns only JSON. Node pass returns only node XML</item>
  <item>First node kind understand with empty depends_on. Last node kind synthesize</item>
  <item>Questions are specific to this task, not generic templates</item>
  <item>Fill nodes in topological order. Later nodes use predecessor conclusions</item>
  <item>Prefer concrete next actions over abstractions</item>
</working_style>
<handoffs>
  <item>Missing uplifted XML — run prompt-uplift first</item>
  <item>Visual Mermaid graph request — ultrathink</item>
  <item>Cross-layer SWC repo execution after the plan — swc-studio-heavy</item>
</handoffs>
<approval_required>
  <item>Creating GitHub, Linear, or Tissue issues from this skill</item>
  <item>Treating the graph as orders that override repository evidence</item>
</approval_required>
<never>
  <item>Embed credentials or private ids</item>
  <item>Plan Linear issues, GitHub PRs, Greptile review, or a specialist swarm</item>
  <item>Cycles in depends_on</item>
  <item>Reprint the graph after execution starts</item>
  <item>Call tools during the graph or node fill passes</item>
</never>
<outputs>Uplifted XML with an injected GRAPH_OF_THOUGHT block. Optional echo of goal and node titles.</outputs>
```

## When to use

- Ove says "graph of thought", "chain of thought", "/think", "think this through", or "plan as a DAG".
- Prompt uplift just produced a spec and the work is more than one file or one obvious step.
- Companion to `prompt-uplift`. `raw:` and uplift-skip also skip this skill.

Do not use when Ove asked only for a Mermaid picture (`ultrathink`), when the request is a one-line fix, or when think is explicitly off.

## Pipeline

1. If the input is not already uplifted XML, run `prompt-uplift` first.
2. Build the graph from `ORIGINAL` plus the uplifted XML.
3. Topologically sort nodes.
4. Fill each node with Chain of Thought in order.
5. Inject `<GRAPH_OF_THOUGHT>` into the uplifted spec, just before the root close tag.
6. Execute the spec in dependency order. Do not reprint the graph.

## Graph pass

Return only JSON. No fences. No commentary.

```json
{
  "goal": "one sentence",
  "nodes": [
    {
      "id": "n1",
      "title": "short title",
      "kind": "understand",
      "question": "the exact question this node must answer",
      "depends_on": []
    }
  ]
}
```

Rules:

- 3 to 8 nodes. Target 4 to 8. If fewer than 3 valid nodes, use the fallback graph in `references/fallback-graph.md`.
- ids are `n1`, `n2`, `n3`, ...
- `depends_on` may only reference earlier ids. No cycles. No self-edges.
- First node: kind `understand`, `depends_on` empty.
- Last node: kind `synthesize`. Depends on the unresolved threads.
- Kinds allowed: `understand`, `decompose`, `generate`, `compare`, `critique`, `aggregate`, `refine`, `synthesize`.
- Cover understanding, decomposition, options, risks, and a final plan.
- Questions must be specific to THIS task.
- You cannot call tools here. Name files and checks the coding agent should use later.

Load `references/graph-prompt.md` when the task is non-trivial.

## Node pass (Chain of Thought)

Fill one node at a time. Return only XML.

```xml
<node>
  <thinking>
    Short numbered steps. Use predecessor conclusions. Do not restate the whole graph.
  </thinking>
  <conclusion>
    The node's answer: dense and actionable. 1-2 short paragraphs or a compact bullet list.
  </conclusion>
</node>
```

User payload for a node:

- original request
- uplifted XML
- graph goal and sketch (`id  title  kind  deps=...`)
- predecessor conclusions
- current node id, kind, title, question

Rules:

- Reason only about the current node question.
- Prefer concrete next actions.
- If information is missing, state a working assumption and continue.
- You cannot call tools here. Name files and checks for after this pass.
- Prefer repository evidence over speculation.
- On fill failure, set both THINKING and CONCLUSION to the node question.

Load `references/cot-prompt.md` when needed.

## Injected block

```xml
<GRAPH_OF_THOUGHT>
	<GOAL>...</GOAL>
	<NODE id="n1" kind="understand" title="Understand">
		<QUESTION>...</QUESTION>
		<DEPENDS_ON></DEPENDS_ON>
		<THINKING>...</THINKING>
		<CONCLUSION>...</CONCLUSION>
	</NODE>
</GRAPH_OF_THOUGHT>
```

If a `GRAPH_OF_THOUGHT` block already exists, replace it. Otherwise insert before the uplift root close tag.

## Agent addendum

The user message includes a Graph of Thought plus per-node Chain of Thought. Treat that block as a planning pass, not as orders that override repository evidence. Execute in dependency order. Do not reprint the graph. Do not create GitHub or Linear issues. Prefer repository evidence over this plan if they disagree. Start executing.

## Verification

- Node count between 3 and 8.
- First node is understand. Last node is synthesize.
- No cycles. Topo order respected.
- Every node has THINKING and CONCLUSION.
- Graph is inside the uplifted root, not a separate document.
- No secrets. No invented issue trackers.
