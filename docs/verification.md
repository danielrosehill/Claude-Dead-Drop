# What is verified here, and what is not

Written 12 August 2026. Kept honest so a later reader knows which claims to lean on.

## Confirmed

**The agent-to-agent primitives exist and behave as documented.** Read directly from the live
`SendMessage` and `ListAgents` tool schemas in a running session, not from release notes. The
contracts in [`cross-session-messaging.md`](cross-session-messaging.md) — addressing by name,
automatic delivery, `<cross-session-message from="...">` wrapping, the enqueue-and-drain model, the
`[ref]` disambiguation rule, the per-session permission boundary — come from those schemas.

**The release facts.** 2.1.224 on 7 August 2026, macOS and Linux, local socket for same-machine
peers, text-only messages, `--name` / `/rename` with folder-name fallback, the 2.1.225 fix to false
"Message sent" reporting. Corroborated across the Claude Code docs and several independent write-ups.

**The prior art.** `sideclaude`, `spawn-planning-repo` and `new-claude-at` were read from
Claude-Hopper's published README on 12 August 2026.

**interrupt-claude is unpublished.** `gh repo view danielrosehill/interrupt-claude-plugin` returns
"Could not resolve to a Repository" — it exists locally with a single scaffold commit and no remote.
Its own `plugin.json` already carries a `repository` URL for that address, which will resolve only
once it is pushed.

## Not verified

**No breakout has been run end to end.** The procedure is a design. The specific failure modes it
claims to prevent are reasoned from the primitives' constraints, not observed.

**The skills have never dispatched.** Their trigger descriptions are untested against a real
session, and they overlap with Hopper's `sideclaude` — which is exactly the condition under which
model dispatch gets unpredictable.

**The socket permissions claim** (`srw-------`) is second-hand from a single report and was not
checked directly.

**Whether the pattern survives contact with more than two concurrent tracks.** interrupt's routing
notes already observe that beyond about two parallel delegates the bottleneck stops being the
machine and becomes the user's attention. Breakout spawns *full sessions*, which are heavier than
subagents, so that ceiling is probably lower here. Untested.

## First things to check when it is used for real

1. Does the spawned session actually appear in `ListAgents` promptly, or is there a registration lag
   that makes the handoff race the spawn?
2. Does the folder-name default produce collisions in practice across sibling repos in the same
   topic group?
3. Does the receiving agent, following `breakout-adopt`, start work without asking an opening
   question?
