# Getting Started with Flutter Claw

## What is Flutter Claw?

Flutter Claw is an open-source agent framework for Flutter. One import gives your app AI agent capabilities: tool calling, memory, multi-modal reasoning — the same patterns as Claude Code and Hermes Agent, but on mobile.

## 30-Second Setup

### 1. Add the dependency

```yaml
# pubspec.yaml

dependencies:
  flutter_claw:
    git:
      url: https://github.com/dan-labs-agi/flutter-claw.git
```

### 2. Create an agent

```dart
import "package:flutter_claw/flutter_claw.dart";

final agent = FlutterClaw(
  model: "claude-sonnet-4-20250514",
  systemPrompt: "You are a helpful assistant. Use tools when needed.",
);

final response = await agent.run(
  input: "What can you see in this photo?",
  attachments: [ImageAttachment(path: "/photos/selfie.jpg")],
);

print(response.text);
// "I see a person smiling at the camera, outdoor setting, sunny."
```

### 3. Add tools

```dart
agent.addTool(ToolDefinition(
  name: "search_web",
  description: "Search the web for information",
  parameters: ToolParameters(
    query: ParameterDefinition(
      type: "string",
      description: "The search query",
    ),
  ),
  execute: (params) async {
    final query = params["query"] as String;
    // Your implementation here
    return "Results for: $query";
  },
));
```

### 4. Enable memory

```dart
agent.enableMemory(
  strategy: MemoryStrategy.summarized,
  maxTokens: 4000,
);
```

## Core Concepts

### Agent

The FlutterClaw agent is your central orchestrator. It manages the conversation loop, decides when to call tools, and maintains memory across turns.

### Tools

Tools are functions the agent can call. Define them with a name, description, parameters, and an execute function. The agent uses the description to decide when to call your tool.

### Memory

Three strategies:
- **full**: Complete conversation history. Best for short sessions.
- **summarized**: Compresses older turns into summaries. Good balance.
- **slidingWindow**: Keeps last N turns verbatim, drops the rest. Most token-efficient.

### Multi-modal

Flutter Claw supports images, audio, and camera input natively. No webview hacks, no platform channels needed.

## Architecture

Flutter Claw is built on three layers:

1. **Sensory input layer** — camera, microphone, IMU. Native Flutter plugins.
2. **Agent orchestration layer** — planning, memory, tool routing. Runs on device.
3. **Action layer** — API calls, notifications, device control. No cloud required.

This architecture is the same one powering the AI glasses at danlab.dev.

## Example: AI Assistant App

```dart
class VoiceAssistantPage extends StatefulWidget {
  @override
  _VoiceAssistantPageState createState() => _VoiceAssistantPageState();
}

class _VoiceAssistantPageState extends State<VoiceAssistantPage> {
  late FlutterClaw _agent;

  @override
  void initState() {
    super.initState();
    _agent = FlutterClaw(
      model: "claude-sonnet-4-20250514",
      systemPrompt: "You are a personal assistant. Respond concisely.",
      enableMemory: true,
      tools: [searchWebTool, calendarTool, reminderTool],
    );
  }

  Future<void> _handleVoiceInput(String transcript) async {
    final response = await _agent.run(input: transcript);
    setState(() => _lastResponse = response.text);
  }
}
```

## Next Steps

- [Full API Reference](docs/api-reference.md)
- [Flutter Claw on GitHub](https://github.com/dan-labs-agi/flutter-claw)
- [danlab.dev](https://danlab.dev)

## Contributing

MIT licensed. Issues, PRs, and feature requests welcome.

Built with Flutter Claw at [dan-labs-agi/flutter-claw](https://github.com/dan-labs-agi/flutter-claw).
