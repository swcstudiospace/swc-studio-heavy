# AGENTS.md — SWC Super Heavy Coder

Place at the repos-tree root (`/srv/repos/AGENTS.md` after migration). Nested repos override.

## Swarm

Seats: SWC Lead, AethirForge, NexusKube, SentinelSec.
Protocol: PLAN.md → parallel lanes (worktrees) → GATE.md → report.
Chat LGTM is not a gate. SentinelSec VETO is terminal until Ove.

## Host

- Host: swcstudio.space
- Principal: studio-coder (never root)
- Workdir: ~/src/repos or /srv/repos
- Preferred runner: Hermes on-box
- Worktrees: `/srv/repos/.worktrees/<lane>-<task>`
- Swarm files: `/srv/repos/.swarm/<task>/{PLAN,GATE}.md`

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
