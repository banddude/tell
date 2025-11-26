# tell

Inter-agent communication for Claude Code instances running in tmux.

## What is this?

Run multiple Claude Code instances as specialized agents that can talk to each other:

```
         ┌─────────────┐
         │     YOU     │
         └──────┬──────┘
                │
         ┌──────▼──────┐
         │  commander  │  ← your main interface
         └──────┬──────┘
                │
     ┌──────────┼──────────┐
     │          │          │
     ▼          ▼          ▼
┌────────┐ ┌────────┐ ┌────────┐
│  work  │ │accounting│ │tech-support│
└────────┘ └────────┘ └────────┘
```

Commander orchestrates. Specialists handle domain tasks. Everyone communicates with `tell`.

## Installation

1. Copy the `tell` script to your PATH:

```bash
cp tell ~/bin/tell
chmod +x ~/bin/tell
```

2. Copy the slash command so agents understand the protocol:

```bash
mkdir -p ~/.claude/commands
cp tell.md ~/.claude/commands/tell.md
```

## Quick Start

### 1. Create your agents

```bash
# Commander (your main interface)
tmux new-session -d -s commander "zsh"
tmux send-keys -t commander "claude" C-m

# Work agent
tmux new-session -d -s work "zsh"
tmux send-keys -t work "claude" C-m

# Add more as needed...
```

### 2. Prime agents with the protocol

```bash
tmux send-keys -t work "/tell" C-m
```

### 3. Attach to commander

```bash
tmux attach -t commander
```

### 4. Start delegating

From commander, send tasks to other agents:

```bash
tell work "check the invoices for this week"
tell accounting "what's the balance for customer ABC"
tell tech-support "the API is returning 502 errors"
```

Agents reply the same way:

```bash
tell commander "found 3 unpaid invoices"
```

## How it works

The `tell` command:
- Auto-detects sender from tmux session name
- Formats messages with sender prefix: `[AGENT MSG FROM WORK]: message`
- Sends to target agent via tmux
- Logs everything to `~/.claude/agent_messages.log`

## Usage

```bash
tell <agent> <message>

# Examples
tell work check the invoices
tell commander "task complete, ready for next"
tell tech-support the OAuth callback is failing
```

## Commands

| Command | Description |
|---------|-------------|
| `tell <agent> <msg>` | Send message to agent |
| `tmux ls` | List running agents |
| `tmux attach -t <agent>` | View an agent's session |
| `cat ~/.claude/agent_messages.log` | View message history |

## Loop Prevention

Agents must NOT reply to closing messages ("thanks", "goodnight", "standing by"). This prevents infinite politeness loops. The protocol doc (`tell.md`) includes this rule.

## Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `TELL_LOG_FILE` | `~/.claude/agent_messages.log` | Message log location |

## Example Agent Network

| Agent | Role |
|-------|------|
| commander | Main orchestrator, talks to user |
| work | Business tasks |
| personal | Personal projects |
| accounting | Invoices, QuickBooks, financials |
| tech-support | Debugging, system admin |
| blogger | Content creation (scheduled) |

## Requirements

- tmux
- Claude Code (`claude` command)
- bash

## License

MIT
