# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

CTF Arena ("Last Container Standing") - AI coding assistants compete in a security CTF. Each agent gets an identical vulnerable Docker container. Agents eliminate opponents by exploiting vulnerabilities and running `kill 1` to stop their containers.

## Commands

```bash
python ctf-arena.py run [agents...]      # Full setup + start agents + attach
python ctf-arena.py setup [agents...]    # Setup without auto-starting agents
python ctf-arena.py start                # Launch AI agents in panes
python ctf-arena.py go                   # Send 'Fight!' to all agents
python ctf-arena.py status               # Show container status
python ctf-arena.py attach               # Attach to tmux session
python ctf-arena.py cleanup              # Stop containers (keeps logins)
python ctf-arena.py cleanup --wipe       # Stop + delete saved logins
```

Example: `python ctf-arena.py run claude codex amp`

## Architecture

- `ctf-arena.py` - Main script. Builds image, creates containers, manages tmux session
- `ctf/Dockerfile` - Vulnerable container with weak passwords, command injection, misconfigured services
- `CTF_RULES.md` - Rules copied to each agent's workspace

### How it works

1. Builds vulnerable container image from `ctf/Dockerfile`
2. Creates isolated Docker network (172.20.0.0/24)
3. Starts one container per agent with persistent `/root` volume
4. Creates tmux session with one pane per agent
5. Death monitor watches containers, turns panes red on elimination

### Adding agents

Edit `ALL_AGENTS` list in `ctf-arena.py`:
```python
{
    "name": "agent-name",
    "container": "ctf-agent-name",
    "run": "agent-cli --flags",
    "env": ["API_KEY_VAR"],
    "color": "colour48",
}
```

API keys are read from `.env` file.

## tmux shortcuts

- `Ctrl+B f` - Send "Fight!" to all agents
- `Ctrl+B q` - Kill session and cleanup containers
