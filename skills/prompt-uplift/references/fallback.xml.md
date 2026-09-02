# Fallback wrap

Use when the rewrite fails, is empty, has no parseable root, or the original exceeds 20000 characters. Escape `& < > "` in ORIGINAL.

```xml
<UPLIFTED_PROMPT>
	<ORIGINAL>ESCAPED_ORIGINAL</ORIGINAL>
	<SYSTEM_ROLE>Senior coding agent. Execute the original request using repository evidence. Do not invent repo-specific facts.</SYSTEM_ROLE>
	<CONTEXT>No additional product context was inferred. Inspect the repository before acting.</CONTEXT>
	<SCOPE>Only the work required to fulfill the original request.</SCOPE>
	<CONSTRAINTS>Do not change unrelated files. Prefer the smallest correct change. Match existing style and architecture. Do not invent paths, library versions, or component names that are not in the request.</CONSTRAINTS>
	<ACCEPTANCE_CRITERIA>The original request is fulfilled. Changes are reviewable and justified.</ACCEPTANCE_CRITERIA>
	<OUT_OF_SCOPE>Unrelated refactors. Speculative features. Invented requirements.</OUT_OF_SCOPE>
	<ASSUMPTIONS>Solve the request in the current workspace using existing patterns unless the original text says otherwise.</ASSUMPTIONS>
</UPLIFTED_PROMPT>
```

Source for this wrap is `fallback`. Allowed roots that skip a second wrap: BUILD_PROMPT, FIX_PROMPT, RESEARCH_PROMPT, CHANGE_PROMPT, UPLIFTED_PROMPT, uplifted, ultrathink.
