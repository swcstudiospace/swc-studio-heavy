# Fallback graph

Use when the planner throws, returns unparseable JSON, or yields fewer than minNodes (default 3) valid nodes.

```json
{
  "goal": "Understand the request, decompose it, weigh approaches, and produce an execution plan",
  "nodes": [
    {
      "id": "n1",
      "title": "Understand",
      "kind": "understand",
      "question": "What is the user actually asking for, including implicit goals and quality bar?",
      "depends_on": []
    },
    {
      "id": "n2",
      "title": "Decompose",
      "kind": "decompose",
      "question": "What subproblems must be solved, in what order, and what does each depend on?",
      "depends_on": ["n1"]
    },
    {
      "id": "n3",
      "title": "Approaches",
      "kind": "generate",
      "question": "What are 2-3 viable approaches and the tradeoffs of each for this specific task?",
      "depends_on": ["n2"]
    },
    {
      "id": "n4",
      "title": "Risks",
      "kind": "critique",
      "question": "What could go wrong: missing constraints, edge cases, regressions, or over-scoping?",
      "depends_on": ["n3"]
    },
    {
      "id": "n5",
      "title": "Plan",
      "kind": "synthesize",
      "question": "What is the concrete execution plan for the coding agent, in order?",
      "depends_on": ["n4"]
    }
  ]
}
```

Normalize rules after any planner output:

- Drop duplicate ids.
- Clamp to maxNodes (default 8).
- Filter depends_on to ids that exist and are not self.
- Force the last remaining node kind to synthesize.
- If still under minNodes, replace with this fallback graph.
