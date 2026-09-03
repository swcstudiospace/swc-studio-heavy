# ADR-003: Live host principal is root@vps.swcstudio.space

Status: Accepted
Date: 2026-09-03
Owners: Ove (risk), SentinelSec (constitution), Grok (docs)

## Context

ADR-001 and ADR-002 documented the agent Unix principal as `studio-coder` and forbade root SSH. The live Hostinger box is reached as `ssh root@vps.swcstudio.space`. Hermes already runs as root. Workspace is `/root/src/repos`. Ove is not creating a `studio-coder` Unix user.

## Decision

1. Product / skill / Bot name stays `swc-studio-coder`. That is not a Unix user.
2. Host is `vps.swcstudio.space`. Unix login is `root`. Workspace is `/root/src/repos`. `/srv/repos` remains a future path.
3. Grok plane is Hermes-on-box (already root). Grok does not open an SSH channel from chat.
4. Laptop Plane B is key-only `ssh root@vps.swcstudio.space` with `IdentityFile ~/.ssh/id_ed25519_swcstudio` (see `vps-swcstudio`). Never password. Never sshpass. Never four SSH sessions.
5. "Never root" as a Unix-user prohibition is retired. Replacement: never open a root SSH channel from Grok or paste a password into chat.
6. Live Grok Bot descriptions stay stale until Ove re-pastes `GROK_BOT_TEAM.md`.

## Consequences

- Docs now match the login. Residual risk is unbounded root on the box.
- Next hardening is a `deploy` sudo user, not inventing `studio-coder` now.
- `HOST_CHECKLIST.md` and `50-studio-coder.conf` in the coder pack stay deferred cutover, not the live principal.

## Alternatives rejected

- Keep writing `studio-coder. Never root.` while the login is root — docs lie.
- Open `ssh root@vps.swcstudio.space` from this Grok seat — extra channel, extra blast radius.
- Write skills onto `/root/.hermes` from chat without an explicit Ove order.
