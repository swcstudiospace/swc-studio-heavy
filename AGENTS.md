# AGENTS.md — SWC Super Heavy Coder

Place at the repos-tree root (`/root/src/repos/AGENTS.md` today; `/srv/repos/AGENTS.md` after migration). Nested repos override.

## Swarm

Seats: SWC Lead, AethirForge, NexusKube, SentinelSec.
Protocol: PLAN.md → parallel lanes (worktrees) → GATE.md → report.
Chat LGTM is not a gate. SentinelSec VETO is terminal until Ove.

## Host

- Host: vps.swcstudio.space
- Unix user: root
- Product / Bot / skill name: swc-studio-coder (not a Unix user)
- Principal: root@vps.swcstudio.space via Hermes-on-box. Grok does not open SSH. Never password. Never sshpass. Never four SSH sessions.
- Workdir: /root/src/repos (same as ~/src/repos for root). /srv/repos is a future path.
- Preferred runner: Hermes on-box (already root)
- Worktrees: `/root/src/repos/.worktrees/<lane>-<task>`
- Swarm files: `/root/src/repos/.swarm/<task>/{PLAN,GATE}.md`

## File ownership

- Application source and lockfiles → AethirForge
- Dockerfile, compose, CI, k8s/helm drafts → NexusKube
- .env, keys, sshd, sudoers → nobody writes; SentinelSec veto zone

## Language defaults (override per repo)

- Python: ruff + pytest
- Rust: cargo fmt + clippy + test
- TypeScript: locked package manager + tsc + vitest/jest
- Elixir: mix format + test
- PHP: composer scripts + pest/phpunit
- Solidity: forge test only, no broadcast
