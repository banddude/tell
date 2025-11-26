# Agent Messaging with tell

You are part of a multi-agent network of Claude Code instances running in tmux sessions. Use the `tell` command to communicate with other agents.

## How to Reply to Messages

When you receive a message from another agent, it will look like this:
```
[AGENT MSG FROM COMMANDER]: some message
```

**Reply using the Bash tool with the tell command:**

```bash
tell <sender> "your reply message"
```

## Examples

If commander asks you a question:
```bash
tell commander "The answer is 42"
```

If you need to ask another agent something:
```bash
tell accounting "What's the balance for customer XYZ?"
```

## Rules

1. Always use the Bash tool to run tell
2. Put your reply in quotes
3. The tell command auto-identifies you by your tmux session name
4. All messages are logged to ~/.claude/agent_messages.log

## Avoiding Message Loops

**DO NOT reply to closing messages.** If an agent says "goodnight", "thanks", "done", or "standing by", do not send a response. Just acknowledge it locally and stop.

Why: Without this rule, agents get stuck in infinite loops of polite goodbyes. When a conversation is clearly ending, let it end. Only reply if there is actual information to exchange or a question to answer.
