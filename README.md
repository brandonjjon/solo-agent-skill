# Solo Skill

An [Agent Skill](https://agentskills.io) for managing local development processes via the [Solo](https://soloterm.com) MCP server.

Solo (SoloTerm) is a desktop app that runs and monitors your dev services — asset bundlers, queue workers, WebSocket servers, log tailers, and more. This skill teaches any compatible agent how to use Solo's MCP tools to start, stop, restart, and inspect those processes on your behalf.

## What it does

- **Start, stop, and restart** individual or all dev processes
- **Check service status** — see which processes are running, stopped, or crashed
- **Read process output** — tail logs, search for errors, check for warnings
- **Diagnose issues** — debug Vite build failures, stuck Horizon workers, WebSocket drops, port conflicts
- **Monitor resources** — check CPU and memory usage across processes

## Installation

Install using any [compatible agent](https://agentskills.io):

```bash
claude skill add --url https://github.com/brandonjjon/solo-skill
```

Or manually copy `SKILL.md` into your agent's skills directory.

## Prerequisites

- A [compatible agent](https://agentskills.io) with MCP support
- [Solo (SoloTerm)](https://soloterm.com) with the MCP server enabled
- A `solo.yml` in your project root defining your processes

## Example usage

```
> are my services running?
> restart Horizon, it seems stuck
> I'm getting a Vite manifest error — can you figure out what's wrong?
> what port is Vite running on?
> stop all the dev services
> check the Reverb logs for connection errors
```

## How it works

The skill provides your agent with:

1. **A workflow** for selecting the correct Solo project before issuing commands
2. **A decision tree** for choosing the right MCP tool based on user intent
3. **Debugging playbooks** for common scenarios (build errors, queue failures, WebSocket issues)
4. **Parameter guidance** so the agent uses the right options (`lines`, `pattern`, `max_results`)

## License

MIT
