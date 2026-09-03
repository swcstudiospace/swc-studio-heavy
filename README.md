# SWC Super Heavy Coder

Four-agent swarm pack. Same seats as the Heavy room. One host identity: root@vps.swcstudio.space via Hermes-on-box. Product name stays swc-studio-coder.

Descriptions use a locked XML tag set. See `SCHEMA.md`. Paste the XML blocks from `GROK_BOT_TEAM.md` into Bot profiles as-is.

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

Invoke:

```
/swc-studio-heavy inventory /root/src/repos. no mutations.
/swc-studio-coder fix the failing test in <repo>
```

## Do not

- Put passwords in any profile
- Open four SSH sessions from Grok, or paste a password into chat
- Treat four Bots as four security tenants
- Write these skills onto `/root/.hermes/skills` as root from chat without an explicit Ove order
