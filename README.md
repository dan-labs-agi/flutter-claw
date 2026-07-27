# Flutter Claw

**Open-source Flutter AI agent framework.** Give any Flutter app a brain — tool calling, memory, and multi-modal reasoning in one import.

[![MIT License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Flutter](https://img.shields.io/badge/Flutter-%2302569B.svg)](https://flutter.dev)

## Why Flutter Claw?

Building an AI agent app in Flutter used to mean weeks of boilerplate. Flutter Claw turns it into a single import.

```dart
import 'package:flutter_claw/flutter_claw.dart';

final agent = ClawAgent(
  system: "You are a helpful assistant with access to the user's apps.",
  tools: [
    CameraTool(),         // native camera + vision
    CalendarTool(),       // read/write device calendar
    ComposioBridge(),     // 1000+ apps via Composio
    MemoryTool(),         // persistent conversation memory
  ],
);
```

That's it. You get:
- Tool calling — same architecture as Claude Code / Hermes Agent
- Memory — conversation history + user context, persistent across sessions
- Multi-modal — camera, mic, on-device vision, voice
- 1000+ apps — bridge to Composio for Gmail, Slack, Notion, Stripe, etc.
- Mobile-first — built for Flutter, not retrofitted

## Quickstart

```bash
flutter pub add flutter_claw
```

```dart
import 'package:flutter_claw/flutter_claw.dart';
import 'package:flutter_claw/composio.dart';

void main() async {
  await Composio.init(apiKey: 'YOUR_COMPOSIO_KEY');

  final agent = ClawAgent(
    tools: [CameraTool(), CalendarTool(), ComposioBridge()],
  );

  final response = await agent.run("What's on my calendar today?");
  print(response.text);
}
```

## Built On

- **Hermes Agent** — graph-loop orchestration, subagent delegation
- **Composio** — 1000+ app integrations with one tool call
- **Open source** — MIT licensed. Use it in your app today.

## Roadmap

- v0.1 — Core agent + tool calling (this release)
- v0.2 — Memory graph + persistent context
- v0.3 — Voice-first loop (STT -> agent -> TTS)
- v0.4 — Graph loop orchestration
- v1.0 — Full Hermes Agent parity + Flutter-native UI

## Built By

**danlab.dev** — AI glasses + agent stack. We are building the future of human-first computing.

## License

MIT © danlab.dev