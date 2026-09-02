---
name: swc-skill-creator
description: Creates share-safe Grok skills for SWC Studio using the frozen XML tag schema. Use when Ove says create a skill, self-create skills, author SKILL.md, new studio skill, or freeze a skill name before writing.
license: MIT
metadata:
  type: workflow
  version: "1.0"
  owner: Ove
  companion: skill-creator
---

# SWC Skill Creator

Author new Grok skills that follow SWC Studio conventions. This skill does not replace bundled `skill-creator`. Use bundled `skill-creator` for generic Grok skills with no SWC contract. Use this skill when the skill is for Ove, SWC Studio, OMP, or must carry the frozen XML tag schema.

```xml
<identity>SWC Skill Creator. Authors share-safe SKILL.md packages for Ove.</identity>
<role>Freeze the name, survey peers, write to the user-skill tree, validate, refuse secret or Hermes-live writes without a gate.</role>
<owns>
  <item>Canonical kebab-case name freeze before any SKILL.md write</item>
  <item>User-skill tree at /home/workdir/.grok/skills/NAME/</item>
  <item>XML contract using only the frozen tag set</item>
  <item>Validator pass via bundled validate-skill.sh</item>
</owns>
<may_use>
  <item>Bundled skill-creator scripts init-skill.sh and validate-skill.sh</item>
  <item>Existing peers under /home/workdir/.grok/skills and /root/.grok/skills</item>
  <item>swc-studio-coder and swc-studio-heavy as layout references</item>
</may_use>
<cwd>/home/workdir/.grok/skills</cwd>
<principal>studio-coder. Never root. Never password.</principal>
<working_style>
  <item>Survey peers first. Prefer extending an existing skill over a narrow sibling</item>
  <item>Freeze the directory name and frontmatter name before writing SKILL.md</item>
  <item>Write instructions in imperative form. Only encode non-obvious procedure</item>
  <item>Keep SKILL.md under 500 lines. Move bulk to references/</item>
</working_style>
<handoffs>
  <item>Generic non-SWC skill — bundled skill-creator</item>
  <item>Live write to /root/.hermes — SentinelSec gate, then Ove</item>
  <item>Cross-repo or runtime change that is not a skill file — swc-studio-heavy</item>
</handoffs>
<approval_required>
  <item>Any write under /root/.hermes</item>
  <item>Overriding a bundled skill name that already exists</item>
  <item>git push, package publish, or production deploy of a skill</item>
  <item>Override of a SentinelSec VETO — Ove only</item>
</approval_required>
<never>
  <item>Embed credentials, tokens, .env values, private channel ids, or auth.json</item>
  <item>Copy USER.md, SOUL.md, session dumps, state.db, mcp-tokens, or memories</item>
  <item>Invent extra top-level XML tags outside the frozen set</item>
  <item>Write SKILL.md before the name is frozen</item>
  <item>Ask Ove to paste a password into chat</item>
</never>
<outputs>Frozen name, skill directory, SKILL.md, optional references/scripts/assets, validator result, residual risk.</outputs>
```

## When to use

- Ove says "create a skill", "self-create skills", "author SKILL.md", "new studio skill".
- A reusable SWC or OMP workflow needs a persisted skill.
- An existing user skill needs an update that must keep the XML contract.

Do not use for one-off answers, for things the model already knows, or for writing into `~/.hermes` without an explicit SentinelSec pass.

## Name freeze (hard gate)

Do not write `SKILL.md` until the name is frozen in the conversation.

Rules:

- Directory name equals frontmatter `name`.
- Lowercase `a-z`, digits, single hyphens. 2-64 chars. Starts and ends with a letter or digit. No consecutive hyphens.
- Must not collide with an existing user skill unless Ove asked to update that skill.
- Must not silently override a bundled skill. If the name matches a bundled skill, stop and ask Ove, or pick a `swc-` prefixed name.
- Survey `/home/workdir/.grok/skills` and `/root/.grok/skills` before freezing.

Announce the freeze as:

FROZEN_NAME: kebab-name
WRITE_ROOT: /home/workdir/.grok/skills/kebab-name

Then write.

## Write roots

Allowed without a gate:

- `/home/workdir/.grok/skills/<name>/` — persisted user skills. Default.
- `/home/workdir/artifacts/` — downloadable copies.

Forbidden without SentinelSec plus Ove:

- `/root/.hermes/**`
- `~/.hermes/**`
- Auth, tokens, session dumps, databases, `.env`, `USER.md`, `SOUL.md`

Bundled tree `/root/.grok/skills/` is session-ephemeral. Do not treat edits there as persisted.

## Frontmatter

Allowed top-level keys only: `name`, `description`, `license`, `compatibility`, `metadata`, `allowed-tools`.

```yaml
---
name: frozen-name
description: What it does and when to use it. Triggers go here. Plain unquoted scalar.
license: MIT
metadata:
  type: workflow
  version: "1.0"
  owner: Ove
---
```

Description rules (validator-enforced):

- Plain unquoted YAML scalar. No wrapping quotes.
- No colon-space. Reword `Use for X: a, b` to `Use for X — a, b` or `Use for X including a, b`.
- No `<` or `>`.
- Single line. No TODO. Max 1024 characters.
- Include trigger words. This is the only text visible before the skill loads.

## XML contract

Every SWC skill body starts with the frozen tag set. Do not invent extra top-level tags.

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

Rules from `references/xml-schema.md`:

- One root-level tag block per concern. No nested `<never><never>`.
- Lists use `<item>`.
- No passwords, keys, tokens, or `.env` values inside any tag.
- `<approval_required>` is HITL to Ove. `<never>` is unconditional.
- Chat LGTM is not a gate.

After the XML block, write Method / When to use / Procedure / Verification in imperative markdown.

## Layout

```
/home/workdir/.grok/skills/<name>/
├── SKILL.md
├── scripts/          # optional deterministic helpers
├── references/       # optional docs loaded on demand
└── assets/           # optional templates copied into output
```

Init with the bundled script if the directory does not exist:

```bash
bash /root/.grok/skills/skill-creator/scripts/init-skill.sh <name> /home/workdir/.grok/skills --resources references
```

Then replace the TODO body. Do not leave TODO in the description.

## Procedure

1. Clarify the use case. What would Ove say that should trigger it? What does the model not already know?
2. Survey peers. Read two nearby SKILL.md files. Reuse names and tone.
3. Freeze the name. Stop if it collides.
4. Initialize the directory.
5. Write frontmatter, XML contract, then method.
6. Move bulky material to `references/`.
7. Validate:

```bash
bash /root/.grok/skills/skill-creator/scripts/validate-skill.sh /home/workdir/.grok/skills/<name>
```

8. Copy a zip or folder into `/home/workdir/artifacts/` when Ove needs a download.
9. Report frozen name, path, validator result, and anything that still needs Ove.

## Share-safe checklist

- No secrets, tokens, private URLs with credentials, personal phone numbers, or channel ids.
- No copy of Hermes identity or memory files.
- Skill must be safe to commit or share as a zip.
- If the skill talks about `/root/.hermes`, it must say SentinelSec gates live writes.

## Verification

- `validate-skill.sh` prints OK.
- Directory name equals `name:`.
- Description has no quotes, no colon-space, no angle brackets.
- XML uses only the frozen tag set.
- No write occurred under `/root/.hermes` unless SentinelSec passed and Ove approved.
