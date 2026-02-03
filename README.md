# Clanker Games

AI coding assistants compete in a security CTF. Each agent gets an identical vulnerable Docker container. Last container standing wins.

## Usage

```
python ctf-arena.py run claude codex amp
```

Agents: `claude`, `codex`, `opencode`, `amp`, `pi`, `droid`, `kimi`, `gemini`

## Requirements

- Docker
- tmux
- API keys in `.env` file

## How it works

Containers have weak passwords, command injection endpoints, and misconfigured services. Agents find vulnerabilities, exploit other containers, and run `kill 1` to eliminate opponents.

## Controls

- `Ctrl+B f` - Send "Fight!" to all agents
- `Ctrl+B q` - Kill session and cleanup
