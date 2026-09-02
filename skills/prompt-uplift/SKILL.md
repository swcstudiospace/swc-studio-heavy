---
name: prompt-uplift
description: Rewrites a sparse user request into a nested XML specification before the agent runs. Use when Ove says uplift, prompt uplift, /uplift, raw prefix, production-grade spec, or wants BUILD_PROMPT FIX_PROMPT RESEARCH_PROMPT CHANGE_PROMPT UPLIFTED_PROMPT density matching the OMP plugin.
license: MIT
metadata:
  type: workflow
  version: "1.0"
  owner: Ove
  source: swcstudiospace/plugin
  companion: graph-of-thought
---

# Prompt Uplift

Expand a short request into a production-grade nested XML spec. Same density as `swcstudiospace/plugin` Prompt Uplift. The rewritten XML is the spec the agent executes. Do not reprint it after uplift unless Ove asks to audit.

```xml
<identity>Prompt Uplift. Expands sparse requests into nested XML specs for a coding agent.</identity>
<role>Classify the work, pick exactly one root tag, expand implicit senior-engineer requirements, keep the user's intent unchanged.</role>
<owns>
  <item>Root selection among BUILD_PROMPT FIX_PROMPT RESEARCH_PROMPT CHANGE_PROMPT UPLIFTED_PROMPT</item>
  <item>Required children ORIGINAL SYSTEM_ROLE CONTEXT-or-APP_CONTEXT SCOPE CONSTRAINTS ACCEPTANCE_CRITERIA OUT_OF_SCOPE</item>
  <item>Domain-shaped nested surfaces named after the work</item>
  <item>Fallback wrap when uplift cannot run</item>
</owns>
<may_use>
  <item>Conversation only as background to recover already-established product facts</item>
  <item>graph-of-thought after a successful uplift</item>
  <item>Repository evidence after the spec is handed to the agent</item>
</may_use>
<cwd>Current workspace. Do not invent paths.</cwd>
<principal>studio-coder when the work is an SWC Studio repo. Otherwise the current agent.</principal>
<working_style>
  <item>Return only XML. First character is the opening tag, last character is the closing tag</item>
  <item>Preserve ORIGINAL verbatim. Conversation is not a new task</item>
  <item>Named nested sections shaped like the work, not a flat generic list and not item soup</item>
  <item>Prefer domain-shaped SCREAMING_SNAKE tags</item>
  <item>Unknowns go in ASSUMPTIONS and AMBIGUITIES, never presented as facts</item>
</working_style>
<handoffs>
  <item>After XML is valid, optionally run graph-of-thought</item>
  <item>Cross-layer SWC repo work after uplift goes to swc-studio-heavy</item>
</handoffs>
<approval_required>
  <item>Inventing repository-specific facts not in the user text or conversation</item>
  <item>Changing the task into adjacent work</item>
</approval_required>
<never>
  <item>Embed credentials, tokens, .env values, or private channel ids</item>
  <item>Invent file paths, library versions, package names, component names, schema tables, or API routes</item>
  <item>Uplift slash commands, trivial acknowledgements, already-uplifted XML, or raw-prefixed text</item>
  <item>Wrap output in markdown fences or add commentary</item>
</never>
<outputs>One rooted XML document. Optional audit line stating root and source llm or fallback.</outputs>
```

## When to use

- Ove says "uplift", "prompt uplift", "/uplift", or "expand this into a spec".
- A sparse request is about to drive implementation and needs production density.
- Prefix `uplift:` forces uplift. Prefix `raw:` skips this skill entirely.

Do not use for slash commands, `ok` / `lgtm` / `thanks`, or text that already starts with an allowed root tag.

## Decide first

Skip when any of these hold:

- Text starts with `/`
- Text starts with `raw:`
- Text is trivial (`yes`, `ok`, `lgtm`, `thanks`, `continue`, `go ahead`)
- Text already starts with `BUILD_PROMPT`, `FIX_PROMPT`, `RESEARCH_PROMPT`, `CHANGE_PROMPT`, `UPLIFTED_PROMPT`, `uplifted`, or `ultrathink`

Otherwise pick exactly one root:

| Root | Use when |
|---|---|
| BUILD_PROMPT | New feature, page, flow, or capability |
| FIX_PROMPT | Bug, regression, broken behavior |
| RESEARCH_PROMPT | Investigate, explain, compare, explore |
| CHANGE_PROMPT | Refactor, rename, migrate, restyle, adjust existing behavior |
| UPLIFTED_PROMPT | None of the above fits cleanly, or fallback wrap |

## Required children

Every root must contain these SCREAMING_SNAKE children:

- `ORIGINAL` — current user request verbatim, not paraphrased, not conversation
- `SYSTEM_ROLE` — coding-agent stance for this work
- `CONTEXT` or `APP_CONTEXT` — `APP_CONTEXT` when the work is a product or app surface
- `SCOPE` — what to implement now
- `CONSTRAINTS` — quality, consistency, hard limits. Always include "do not invent repo facts"
- `ACCEPTANCE_CRITERIA` — observable, testable outcomes
- `OUT_OF_SCOPE` — adjacent work a junior might start by accident

Add only when relevant. Do not emit empty stubs:

- `PLATFORM_CONSTRAINTS`
- `DESIGN_SYSTEM_CONTINUITY`
- `SECURITY_AND_VALIDATION`
- `GRACEFUL_DEGRADATION`
- `STATES`
- `WORKFLOW`
- `ASSUMPTIONS`
- `AMBIGUITIES`
- Nested feature sections matching named surfaces in the request (tabs, pages, modals, APIs, jobs)

## Gold pattern

Named nested sections shaped like the work (`CLIENTS_LIST_PAGE`, `ADD_EDIT_CLIENT_MODAL`), not a flat generic list.

Expand implicit requirements a senior engineer would assume: quality bar, loading/empty/error/success states, edge cases, validation, security, accessibility (keyboard, focus, labels, contrast, reduced-motion), graceful degradation, retries, operational failure modes. Be concrete and operational, not motivational.

Do not change the task. Do not add adjacent products. Do not "improve" the request into different work.

## Shape hint

Replace `NAMED_SURFACE` with real domain names from the request.

```xml
<BUILD_PROMPT>
  <ORIGINAL>...</ORIGINAL>
  <SYSTEM_ROLE>...</SYSTEM_ROLE>
  <APP_CONTEXT>...</APP_CONTEXT>
  <SCOPE>...</SCOPE>
  <CONSTRAINTS>...</CONSTRAINTS>
  <NAMED_SURFACE>
    <SUBSECTION>...</SUBSECTION>
    <STATES>...</STATES>
  </NAMED_SURFACE>
  <SECURITY_AND_VALIDATION>...</SECURITY_AND_VALIDATION>
  <GRACEFUL_DEGRADATION>...</GRACEFUL_DEGRADATION>
  <ACCEPTANCE_CRITERIA>...</ACCEPTANCE_CRITERIA>
  <OUT_OF_SCOPE>...</OUT_OF_SCOPE>
  <ASSUMPTIONS>...</ASSUMPTIONS>
  <AMBIGUITIES>...</AMBIGUITIES>
</BUILD_PROMPT>
```

## Fallback

If the rewrite would fail, is empty, or exceeds 20000 characters, wrap with the conservative fallback in `references/fallback.xml.md`. Root becomes `UPLIFTED_PROMPT`. Source is `fallback`.

## Agent addendum

After handing the XML to the executing agent, treat this as standing instruction and do not reprint the spec:

The user message is an uplifted XML specification. Treat it as the source of truth. Execute it. Do not reprint the XML. Prefer repository evidence over inferred assumptions.

## Procedure

1. Parse prefixes. `raw:` returns the remainder unchanged. `uplift:` forces a rewrite.
2. Apply skip rules.
3. Classify the work and pick one root.
4. Copy ORIGINAL verbatim.
5. Expand into named nested sections. Load `references/system-prompt.md` if the rewrite is non-trivial.
6. If the result has no allowed root, wrap in `UPLIFTED_PROMPT`. If it lacks `ORIGINAL`, insert it as the first child.
7. Optionally hand the XML to `graph-of-thought`.
8. Execute the spec. Do not reprint the XML unless Ove asks `/uplift last` or an audit.

## Verification

- Output starts with `<` and ends with `>`.
- Exactly one allowed root.
- `ORIGINAL` matches the user request verbatim.
- No invented repo facts.
- No secrets.
- Nested tags are domain-shaped, not a 10-field template.
