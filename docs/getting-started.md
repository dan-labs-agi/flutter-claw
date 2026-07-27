# Getting Started — Flutter Claw

## Prerequisites

- **Flutter SDK** 3.24+ ([flutter.dev](https://flutter.dev/docs/get-started/install))
- **Dart** 3.5+ (bundled with Flutter SDK)
- **Git** 2.30+

## 5-Minute Quickstart

### 1. Clone the template

```bash
git clone https://github.com/dan-labs-agi/flutter-claw.git
cd flutter-claw
```

### 2. Install dependencies

```bash
flutter pub get
```

### 3. Add your API keys

```bash
cp .env.example .env
```

Edit `.env` with your keys:

```
OPENAI_API_KEY=sk-...
ANTHROPIC_API_KEY=sk-ant-...
COMPOSIO_API_KEY=csk_...
```

### 4. Run the agent

```bash
flutter run -d chrome
```

The agent UI opens at `http://localhost:8080`.

## First Command

Type anything in the chat. Example:

> "Fetch my unread emails and summarize the top 3"

The agent routes to Hermes + Composio, fetches your Gmail via the Composio toolkit, and returns a structured summary — all from a single Flutter UI.

## Architecture in 30 Seconds

```
Flutter UI  →  Hermes Agent  →  Composio  →  1000+ apps
                                     (Gmail, Slack, GitHub, Stripe…)
```

Hermes handles reasoning and tool orchestration. Composio handles auth + API calls. Flutter handles presentation and mobility.

## Extending

Edit `lib/agent/agent_channel.dart` to add custom tools or swap the model.

## License

MIT — use it in personal and commercial projects. Star the repo if it helped you build fast.
