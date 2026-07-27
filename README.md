# Flutter Claw

Production-ready Flutter AI agent app template with Hermes Agent, Composio (1000+ apps), and graph loop architecture. MIT-licensed.

## What is Flutter Claw?

Flutter Claw is a complete Flutter application that puts a full AI agent system in your pocket. Built on [Hermes Agent](https://hermes-agent.nousresearch.com) and [Composio](https://composio.dev) for 1000+ app integrations.

## Features

- **Hermes Agent Integration** - Full skill system, graph loops, 24/7 daemon
- **Composio Integration** - 1000+ app connections (Gmail, LinkedIn, Slack, Notion, GitHub, etc.)
- **Graph Loop Architecture** - DISCOVER -> STRATEGIZE -> EXECUTE -> MEASURE -> OPTIMIZE autonomous loops
- **Subagent Delegation** - Parallel task execution via Hermes delegation
- **Obsidian Memory Graph** - Persistent knowledge graph for agent memory
- **Telegram Gateway** - Remote control and monitoring via Telegram

## Quick Start

### Prerequisites
- Flutter 3.x / Dart 3.x
- Python 3.11+
- Composio account (free tier works)

### Setup

1. Clone this repo
2. Install dependencies
3. Configure Composio
4. Run the app

See docs/SETUP.md for full instructions.

## Architecture

```
Flutter App (UI/UX)
    |
    |  stdio MCP bridge
    |
Hermes Agent (Python backend)
    |
    |  Composio CLI
    |
1000+ Apps (Gmail, LinkedIn, GitHub, etc.)
```

## License

MIT - see LICENSE

## Built by

[DanLab](https://danlab.dev) - AI agents that run your business while you sleep.