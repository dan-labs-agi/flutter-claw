---
title: "How to Build an AI Agent with Tool Calling in Flutter"
date: 2026-07-28
author: danlab.dev
tags: [flutter, ai-agents, tool-calling, tutorial, hermes-agent]
canonical: https://danlab.dev/blog/flutter-ai-agent-tool-calling
description: "Step-by-step tutorial: build a multi-tool AI agent in Flutter using Flutter Claw. Camera, calendar, Gmail, persistent memory — all in one Dart file."
---

# How to Build an AI Agent with Tool Calling in Flutter

This is the tutorial I wish existed when I started building AI agents in Flutter. By the end, you'll have a working agent that:

- Sees through the device camera (vision)
- Reads and writes the device calendar
- Sends email via Gmail (through Composio)
- Remembers previous conversations across app restarts

The whole thing is one Dart file and ~50 lines.

## What is Tool Calling?

Tool calling is the pattern where an LLM doesn't just generate text — it decides which functions to invoke, fills in the arguments, and uses the results to compose its final answer. The classic loop:

```
user message → LLM → tool calls → tool results → LLM → final answer
```

Claude, GPT-4, and Gemini all support this. The hard part is wiring it to your app's actual APIs.

## Why Mobile Agents Are Different

Desktop agents have an easy life. Files, terminals, browsers — all accessible. Mobile agents have:

- **Latency constraints** — 4G networks, not ethernet
- **Background suspension** — your agent has to be resumable
- **Native APIs only** — no shell access, no file system
- **Permission prompts** — camera, mic, calendar, location all require user consent
- **Sandboxing** — apps can't see each other, but they CAN talk to system services

Flutter Claw wraps all of this in a single, Flutter-idiomatic API.

## Step 1: Add the package

```bash
flutter create my_agent_app
cd my_agent_app
flutter pub add flutter_claw
```

## Step 2: Configure the agent

```dart
import 'package:flutter_claw/flutter_claw.dart';
import 'package:flutter_claw/composio.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Composio.init(apiKey: 'YOUR_COMPOSIO_KEY');

  final agent = ClawAgent(
    system: "You are a personal AI assistant with vision, calendar, and email.",
    tools: [
      CameraTool(),
      CalendarTool(),
      LocationTool(),
      ComposioBridge(),
    ],
    memory: PersistentMemory(database: 'agent.db'),
  );

  runApp(MyApp(agent: agent));
}
```

## Step 3: Wire to a chat UI

```dart
class ChatScreen extends StatefulWidget {
  final ClawAgent agent;
  const ChatScreen({required this.agent, super.key});

  @override
  State<ChatScreen> createState() => _ChatScreenState();
}

class _ChatScreenState extends State<ChatScreen> {
  final _messages = <ChatMessage>[];
  final _controller = TextEditingController();

  Future<void> _send() async {
    final text = _controller.text;
    _controller.clear();

    setState(() => _messages.add(ChatMessage.user(text)));

    final response = await widget.agent.run(
      userMessage: text,
      images: await _captureIfRequested(text),
    );

    setState(() => _messages.add(ChatMessage.assistant(response.text)));
  }

  Future<List<CameraImage>?> _captureIfRequested(String text) async {
    // Simple heuristic; in production, the agent itself requests the camera
    if (text.toLowerCase().contains('see this') ||
        text.toLowerCase().contains('what is this')) {
      return await CameraController().takePicture();
    }
    return null;
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('My Agent')),
      body: Column(
        children: [
          Expanded(
            child: ListView.builder(
              itemCount: _messages.length,
              itemBuilder: (_, i) => ChatBubble(message: _messages[i]),
            ),
          ),
          Padding(
            padding: const EdgeInsets.all(8),
            child: Row(
              children: [
                Expanded(
                  child: TextField(controller: _controller),
                ),
                IconButton(
                  icon: const Icon(Icons.send),
                  onPressed: _send,
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }
}
```

## Step 4: Try the agent

```dart
// In your main app or a button handler
final response = await agent.run(
  "Schedule lunch with Sarah tomorrow at noon and email her the details.",
);
print(response.text);
```

The agent will:
1. Look at your calendar for tomorrow at noon
2. Pick an open slot
3. Find Sarah's email in your contacts (via Composio)
4. Compose and send the email via Gmail (via Composio)
5. Add the event to your calendar
6. Return a summary

All multi-step. All tool calling. All on a phone.

## Production Tips

- **Token caching** — `PersistentMemory` already does this; don't double-cache.
- **Error handling** — every tool call can fail. `ClawAgent.run` returns an `AgentResult` with `success`, `error`, and `text` fields.
- **Permission UX** — request camera/mic/calendar permissions on first use, not on app open.
- **Rate limiting** — Composio has its own limits; check `agent.lastUsage` for current spend.
- **Background work** — use `WorkManager` for agents that need to run while the app is suspended.

## Get the Code

The full tutorial code is in the [Flutter Claw GitHub repo](https://github.com/dan-labs-agi/flutter-claw) under `examples/calendar_agent/`.

⭐ Star the repo to follow the series — next article covers graph-loop orchestration (one agent delegating to subagents for parallel work).

---

**About danlab.dev:** We build AI glasses and the open agent stack underneath them. Flutter Claw is the mobile layer; the glasses hardware is the deployment surface. We're raising pre-seed and shipping weekly. founders@danlab.dev.
