# ADR-002: Four-agent Super Heavy swarm for SWC Studio Coder

Status: Accepted
Date: 2026-09-02
Owners: Grok (synthesis), SentinelSec (constitution), NexusKube (topology), AethirForge (language lanes)

## Context

Ove asked to set up the Coder like Grok Super Heavy with 4 agents that swarm. The previous ADR rejected password-root SSH and shipped a single Staff Engineer Bot. This ADR splits that Bot into four isomorphic seats without multiplying host identities.

## Decision

1. Roster is isomorphic with the Heavy room: SWC Lead, AethirForge, NexusKube, SentinelSec.
2. Four roles, one principal (`studio-coder` / Hermes-on-box). Not four SSH sessions.
3. Grok Bot group chat for standing teammates. Grok Build subagents + worktrees for actual parallel edits.
4. SentinelSec `GATE.md` is the only merge gate. Chat LGTM is not a gate. VETO is not overridable by the other three seats.
5. Depth 1 only. Lead fans out. Children do not spawn children.
6. Do not swarm single-file fixes.

## Consequences

- Token cost and latency rise on cross-layer work; quality and blast-radius control rise with them.
- Shared Grok Bot computer remains one security boundary. Role text cannot fix that. Keys stay off that VM except as a dedicated studio-coder identity.
- File collisions become a process bug. Lead must assign non-overlapping slices.

## Alternatives rejected

- One generalist Coder with "pretend swarm" in a single prompt — no separation of duty.
- Four root SSH identities — SentinelSec veto.
- 16-agent research swarm — wrong product surface for repo mutation.
- Letting Forge override Sentinel after two debate rounds — repudiation and priv-esc via consensus.
