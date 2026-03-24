---
name: solo
description: >-
  Manages local development processes (Vite, Horizon, Reverb, Pail, etc.) via the Solo MCP server.
  Use this skill when the user wants to start, stop, restart, or check the status of local dev services;
  read or search process output/logs from running services; check which ports processes are using;
  investigate CPU or memory usage of dev processes; or mentions Solo, SoloTerm, or solo.yml.
  Also activate when diagnosing local build failures (e.g. "CSS not updating", "Vite manifest error"),
  local WebSocket connection drops (checking Reverb output), stuck local queue workers (checking Horizon output),
  or any problem where reading a running dev process's stdout/stderr would help — even if the user
  doesn't mention Solo by name. Do NOT use for: configuring or installing Horizon/Reverb/Pulse (use those
  dedicated skills instead), reading Laravel log files (laravel.log), production server issues,
  or application code changes.
license: MIT
allowed-tools: mcp__solo__list_projects mcp__solo__select_project mcp__solo__list_processes mcp__solo__get_project_status mcp__solo__get_project_stats mcp__solo__start_process mcp__solo__stop_process mcp__solo__restart_process mcp__solo__start_all_processes mcp__solo__stop_all_processes mcp__solo__restart_all_processes mcp__solo__get_process_output mcp__solo__search_output mcp__solo__clear_output mcp__solo__get_process_ports mcp__solo__get_process_status mcp__solo__setup_agent_integration
metadata:
  version: 0.2.0
---

# Solo Dev Process Management

Solo (SoloTerm) is a desktop app that manages development processes like asset bundlers, queue workers, and WebSocket servers. It exposes MCP tools (prefixed `mcp__solo__`) that let you control and inspect these processes without the user needing to switch to a terminal.

## Getting Started

Before using any process tools, you need an active project selected:

1. Call `list_projects` — if only one project exists, it auto-selects
2. If multiple projects exist, call `select_project` with the correct `project_id`

You only need to do this once per session. All subsequent calls operate on the selected project.

## Choosing the Right Tool

**To understand what's running:**
- `get_project_status` — overview of all processes and their states (start here)
- `list_processes` — list process names and configurations
- `get_project_stats` — CPU and memory usage, sorted by memory (use when investigating performance)

**To control processes:**
- `start_process` / `stop_process` / `restart_process` — act on a single process by name
- `start_all_processes` / `stop_all_processes` / `restart_all_processes` — act on everything at once

**To read output:**
- `get_process_output` — recent stdout/stderr (default 50 lines; pass `lines` param for more)
- `search_output` — find lines matching a pattern (case-insensitive substring; pass `max_results` to control volume)
- `clear_output` — flush a process's output buffer

**Other:**
- `get_process_ports` — which TCP ports a process is listening on
- `setup_agent_integration` — add Solo docs to the project's CLAUDE.md

## Common Workflows

### Debugging build errors
When the user reports a frontend issue or you see a Vite manifest error:
1. `get_process_output` on the bundler process (usually "Vite")
2. `search_output` for "error", "failed", or "warning" if the output is long
3. Fix the issue, then `restart_process` to pick up changes

### Debugging queue / job failures
1. `get_process_output` on the queue worker (usually "Horizon")
2. `search_output` for "fail", "exception", or the specific job class name

### Debugging WebSocket issues
1. `get_process_output` on the WebSocket server (usually "Reverb")
2. Check for connection errors, auth failures, or port conflicts with `get_process_ports`

### Checking if services are healthy
1. `get_project_status` for a quick overview
2. If a process shows as stopped unexpectedly, read its output for crash details before restarting

## Important Constraints

- Processes must be **trusted in the Solo UI** before they can be started via MCP. If `start_process` fails, tell the user to trust the process in Solo first.
- Solo can read output but **cannot send interactive input** to processes.
- Process names are defined in the project's `solo.yml` — read it if you need the exact names.
