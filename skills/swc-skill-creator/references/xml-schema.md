# Frozen SWC description schema

Every Bot profile, CLI agent body, and SWC skill XML contract uses this tag set. Do not invent extra top-level tags.

```xml
<identity>...</identity>
<role>...</role>
<owns>...</owns>
<may_use>...</may_use>
<cwd>...</cwd>
<principal>...</principal>
<working_style>...</working_style>
<handoffs>...</handoffs>
<approval_required>...</approval_required>
<never>...</never>
<outputs>...</outputs>
```

Rules

- One root-level tag block per concern. No nested never-inside-never.
- Lists use item tags.
- No passwords, keys, tokens, or .env values inside any tag.
- approval_required is HITL to Ove. never is unconditional.
- A verbal LGTM is not a gate. Only GATE.md PASS from SentinelSec unblocks merge.

Where this schema is pasted

- Single-seat Bot description
- Four swarm Bot descriptions
- Grok Build CLI agents
- Skills under skills/*/SKILL.md as XML contract then Method

This is the skill-body contract. It is separate from Prompt Uplift roots (BUILD_PROMPT, FIX_PROMPT, RESEARCH_PROMPT, CHANGE_PROMPT, UPLIFTED_PROMPT), which belong to the prompt-uplift skill.
