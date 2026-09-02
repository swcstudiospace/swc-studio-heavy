# Authoring rules (Grok + SWC)

## Grok validator (hard fail)

Mirrors bundled validate-skill.sh.

- SKILL.md exists and starts with ASCII `---`
- name is 2-64 chars, `^[a-z0-9](?:[a-z0-9]|-(?!-))*[a-z0-9]$`, equals directory name
- description required, non-empty, ≤1024 chars, no leftover TODO
- description is a plain unquoted YAML scalar
- description contains no colon-space and no angle brackets
- compatibility if set is ≤500 chars and unquoted
- frontmatter only agentskills.io fields: name, description, license, compatibility, metadata, allowed-tools
- body is non-empty
- no tokenizer control tokens shaped like pipe-bracket reserved ids

## SWC additions

- Freeze name before writing SKILL.md
- Include the frozen XML tag set in the body
- Persist under /home/workdir/.grok/skills only
- Share-safe. No Hermes live writes without SentinelSec
- Do not override bundled skill names without Ove
- Keep SKILL.md under 500 lines
- Do not create README.md or CHANGELOG.md inside the skill unless Ove asks for a human pack

## Init and validate

```bash
bash /root/.grok/skills/skill-creator/scripts/init-skill.sh <name> /home/workdir/.grok/skills --resources references
bash /root/.grok/skills/skill-creator/scripts/validate-skill.sh /home/workdir/.grok/skills/<name>
```
