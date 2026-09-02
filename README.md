# SWC Super Heavy Coder

Four-agent swarm pack. Same seats as the Heavy room. One host identity.

Descriptions use a locked XML tag set. See `SCHEMA.md`. Paste the XML blocks from `GROK_BOT_TEAM.md` into Bot profiles as-is.

## Grok App vs Grok Bot

| Surface | Public command |
|---|---|
| Grok App slash picker | `/ultrathink` only |
| Grok Bot / Hermes | `swc-studio-coder`, `swc-studio-heavy`, `swc-skill-creator` |

`prompt-uplift` and `graph-of-thought` are retired libraries. Both pipelines live inside `skills/ultrathink` Stages 1-3. Do not invoke them.

## Install

### Grok Bot (standing teammates)

1. Create four Bots from `GROK_BOT_TEAM.md`.
2. Open group chat **SWC Super Heavy Coder**.
3. Paste the standing-order XML from that file.
4. Send the inventory-only first task as a message, not as profile text.

### Grok Build CLI (real parallel edits)

```bash
mkdir -p ~/.grok/agents ~/.grok/personas ~/.grok/skills/swc-studio-heavy ~/.grok/skills/swc-studio-coder
cp agents/*.md ~/.grok/agents/
cp personas/*.toml ~/.grok/personas/
cp skills/swc-studio-heavy/SKILL.md ~/.grok/skills/swc-studio-heavy/
cp skills/swc-studio-coder/SKILL.md ~/.grok/skills/swc-studio-coder/
```

Bot / CLI invoke by name, not as Grok App slash commands:

```
swc-studio-heavy inventory ~/src/repos. no mutations.
swc-studio-coder fix the failing test in <repo>
```

### Grok App

Install only `skills/ultrathink`. Invoke:

```
/ultrathink <task>
```

## Do not

- Put passwords in any profile
- Open four root SSH sessions
- Treat four Bots as four security tenants
- Write these skills onto `/root/.hermes/skills` as root from chat without an explicit Ove order
- Publish SWC skills as Grok App slash commands
