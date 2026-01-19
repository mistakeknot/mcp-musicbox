# MCP MusicBox

> See `AGENTS.md` for full development guide and architecture details.

## Overview

MCP server bridging Claude Desktop with Sonic Pi for live coding music via OSC.

## Status: Development

Single-file FastMCP server using python-sonic (psonic).

## Quick Commands

```bash
uv sync                          # Install dependencies
uv run mcp-musicbox/server.py    # Run MCP server
uv run ruff check .              # Lint
uv run ruff format .             # Format
```

## Key Files

- `mcp-musicbox/server.py` - Single-file FastMCP server
- `SHARED_STATE_PATH` env var - Live parameters JSON

## Design Decisions (Do Not Re-Ask)

- **python-sonic (psonic)**: Established library for Sonic Pi communication
- **Single-file server**: Keep simple, no complex module structure needed
- **Time State for live mix**: Enables parameter changes without stopping music
- **Parse daemon.log**: Extract connection params from Sonic Pi's log file
