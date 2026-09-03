# Paste-ready XML descriptions

Use the XML fences in `GROK_BOT_TEAM.md` for the four Bots and the standing order.
Use `../swc-studio-coder/GROK_BOT_PROFILE.md` if you keep a single-seat Coder instead of renaming it to SWC Lead.

Schema (do not invent extra top-level tags):

```xml
<identity></identity>
<role></role>
<owns></owns>
<may_use></may_use>
<cwd></cwd>
<principal></principal>
<working_style></working_style>
<handoffs></handoffs>
<approval_required></approval_required>
<never></never>
<outputs></outputs>
```

`<approval_required>` is HITL to Ove. `<never>` is unconditional.
First-task inventory text is a chat message, not a profile tag.

Live principal (ADR-003): `root@vps.swcstudio.space` via Hermes-on-box. Product name stays `swc-studio-coder`. Grok does not open SSH. Never password in chat. Never sshpass. Never four SSH sessions. Re-paste Bot profiles from `GROK_BOT_TEAM.md` after this change.
