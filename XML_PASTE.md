# XML paste pack — SWC descriptions

Shared schema (do not invent tags):

`<identity>` `<role>` `<owns>` `<may_use>` `<cwd>` `<principal>` `<working_style>` `<handoffs>` `<approval_required>` `<never>` `<outputs>`

Rules: lists use `<item>`. No secrets in any tag. First tasks are messages, not description text.

## Where to paste

| Surface | File | Block |
|---|---|---|
| Single-seat Bot description | `../swc-studio-coder/GROK_BOT_PROFILE.md` | the ```xml block |
| Four swarm Bot descriptions + standing order | `GROK_BOT_TEAM.md` | five ```xml blocks |
| Grok Build CLI agents | `agents/*.md` | YAML frontmatter + XML body |
| Skills | `skills/*/SKILL.md` | XML contract then Method |

Grok Bot Description is plain text. Tags are stored as text. Each Bot block is ~2.0–2.3k characters.

## Security contract (unchanged)

Chat LGTM is not a gate. Only GATE.md PASS from SentinelSec unblocks merge. VETO is terminal until Ove. One principal: studio-coder. Never password. Never root. Never four SSH sessions.
