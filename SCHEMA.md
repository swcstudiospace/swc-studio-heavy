# Description schema (XML tags)

Every Bot profile and CLI agent body uses this tag set. Do not invent extra top-level tags.

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
- One root-level tag block per concern. No nested `<never><never>`.
- Lists use `<item>`.
- No passwords, keys, tokens, or `.env` values inside any tag.
- `<approval_required>` is HITL to Ove. `<never>` is unconditional.
- A verbal LGTM is not a gate. Only `<outputs>` GATE.md PASS from SentinelSec unblocks merge.
