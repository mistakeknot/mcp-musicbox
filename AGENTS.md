# MCP MusicBox - Development Guide

> MCP server bridging Claude Desktop with Sonic Pi for live coding music.

## Architecture

Single-file FastMCP server (`mcp-musicbox/server.py`) using python-sonic (psonic) to send OSC messages to Sonic Pi's daemon.

### MCP Tools

| Tool | Purpose |
|------|---------|
| `initialize_sonic_pi()` | Start Sonic Pi app and establish connection |
| `reconnect_sonic_pi()` | Reconnect without restart (session recovery) |
| `play_music(code)` | Execute Sonic Pi Ruby code |
| `stop_music()` | Stop all audio |
| `change_mix(parameters)` | Update live mix via Time State |
| `read_shared_state()` | Read current parameter values |
| `debug_sonic_pi_connection()` | Show connection diagnostics |

### Connection Flow

1. Server parses `~/.sonic-pi/log/daemon.log` to extract:
   - Daemon token
   - GUI port (`gui-send-to-spider`)
   - OSC port (`osc-cues`)
2. Calls `psonic.set_server_parameter()` with values
3. Uses `psonic.run()` to send code, `psonic.stop()` to halt

### Live Mix System

Parameters stored in JSON file (path via `SHARED_STATE_PATH`) and sent to Sonic Pi via Time State (`set :param, value`). Enables real-time effect control without stopping music.

## Development Commands

```bash
# Install dependencies
uv sync

# Run the MCP server (development)
uv run mcp-musicbox/server.py

# Lint code
uv run ruff check .

# Format code
uv run ruff format .
```

## Configuration

| Variable | Purpose | Default |
|----------|---------|---------|
| `SONIC_PI_APP_PATH` | macOS app location | `/Applications/Sonic Pi.app` |
| `SHARED_STATE_PATH` | Live parameters JSON | Must be configured |

## Dependencies

- `mcp` - Model Context Protocol SDK
- `python-sonic` (psonic) - Python-to-Sonic Pi bridge
- `python-osc` - OSC protocol support
- `ruff` - Linting and formatting
