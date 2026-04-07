# Agentix

> Open source visual builder for AI agents. No code. No token middleman. Runs local with **Ollama** (free) or with the **Anthropic API** (your key, your tokens). The builder is yours forever.

![status](https://img.shields.io/badge/status-v0.1.0--alpha-b6ff3c)
![license](https://img.shields.io/badge/license-MIT-b6ff3c)
![price](https://img.shields.io/badge/price-free%20forever-b6ff3c)
![ollama](https://img.shields.io/badge/ollama-supported-b6ff3c)

---

## Why Agentix?

Every "AI agent platform" out there (Zapier, Make, n8n cloud, Relevance, etc.) charges you a fat margin on top of the tokens you'd pay the LLM provider directly. They host your logic on their servers and lock you in.

**Agentix flips that.** The builder is a single HTML file that runs entirely in your browser. You bring your own LLM — either **Ollama running locally** (totally free, your hardware) or your own **Anthropic API key**. We never see anything, we never run a server, we never lock you in.

The product is dead simple:

- **Builder** → free, MIT, runs locally, forever
- **Marketplace** → buy and sell pre-built agents (coming soon)
- **Cloud** → optional hosted execution if you want webhooks/cron (later)

---

## Getting started

You don't need to install anything to use the builder. Seriously.

```bash
git clone https://github.com/MiguelAngelDiRocco/agentix.git
cd agentix
open builder.html   # or just double-click it
```

Or even simpler: download `builder.html` and open it in any modern browser.

### Setting up Ollama (recommended — free, local, no API costs)

Ollama runs LLMs locally on your machine. Free forever, your hardware does the work.

**1. Install Ollama** → download from [ollama.com](https://ollama.com) (Mac, Linux, Windows)

**2. Pull a model** (in your terminal):
```bash
ollama pull llama3.2:3b      # 2.5GB · recommended for 16GB RAM Macs
# or
ollama pull qwen2.5:7b       # 5GB · smarter, needs more RAM
ollama pull llama3.1:8b      # 5GB · also good
ollama pull phi3.5           # 2.8GB · fast and capable
```

**3. Enable CORS so the browser can talk to Ollama** (one-time setup):

On **macOS**:
```bash
launchctl setenv OLLAMA_ORIGINS "*"
```
Then quit and restart the Ollama app from the menu bar.

On **Linux**:
```bash
# Edit the systemd service
systemctl edit ollama.service
# Add under [Service]:
Environment="OLLAMA_ORIGINS=*"
# Then:
systemctl daemon-reload && systemctl restart ollama
```

On **Windows**: set the `OLLAMA_ORIGINS` environment variable to `*` in System Properties → Environment Variables, then restart Ollama.

**4. Open the builder**, click **⚙ Provider**, select Ollama, pick your model, and you're set.

> Without `OLLAMA_ORIGINS=*` the browser will block the request with a CORS error. This is the most common gotcha.

### Setting up Anthropic API (optional — paid per token)

If you want top-tier quality (Sonnet/Opus) for production agents:

1. Go to [console.anthropic.com](https://console.anthropic.com), create a key, add a few dollars of credit
2. Open the builder, click **⚙ Provider**, select "Anthropic API", paste the key
3. Pick a model and run

> Note: an Anthropic Pro subscription ($20/mo for claude.ai) does **not** include API access. They're separate billings.

### First agent in 60 seconds

1. Drag a **Trigger** node onto the canvas
2. Drag a **Claude LLM** node next to it
3. Drag an **Output** node after that
4. Connect them: click and drag from the right port of one to the left port of the next
5. Click each node and fill in the configuration
6. Hit **▶ Run**

That's it. You just built your first agent.

---

## Node types (v0.1.0)

| Node | What it does |
|------|--------------|
| **Trigger** | The starting point. Holds the initial input text. |
| **Claude LLM** | Sends a system + user prompt to Claude. Use `{{input}}` to reference the previous node's output. |
| **HTTP Request** | Call any REST API. GET / POST / PUT / DELETE. |
| **Condition** | If output contains X, branch to one of two values. |
| **Output** | Marks the final result of the agent. |

More nodes coming: Slack, Email, Database, Webhook, Schedule, Loop, Parser.

---

## How it works under the hood

```
┌───────────────────┐
│   builder.html    │  ← single file, no build, no backend
│  (runs in browser)│
└─────────┬─────────┘
          │
          ├──────────────────┐
          │                  │
          ↓                  ↓
┌──────────────────┐  ┌──────────────────┐
│  Ollama (local)  │  │  Anthropic API   │
│ localhost:11434  │  │ api.anthropic.com│
│   (free, your    │  │  (paid, your     │
│    hardware)     │  │     key)         │
└──────────────────┘  └──────────────────┘
```

**That's the whole architecture.** No database. No servers. No accounts. No telemetry.

When you "save" an agent, it exports as a `.agentix.json` file. When you "load" one, it reads that file. Your work is yours.

---

## Roadmap

### v0.1.0 — MVP (you are here)
- [x] Visual canvas with drag-drop nodes
- [x] 5 node types (Trigger, Claude LLM, HTTP, Condition, Output)
- [x] Live execution in browser
- [x] Import / export as JSON
- [x] Local API key storage

### v0.2.0 — Polish
- [ ] Multiple inputs per node
- [ ] Better edge routing (no crossings)
- [ ] Pan and zoom canvas
- [ ] Template gallery (10 starter agents)
- [ ] Dark / light themes

### v0.3.0 — Marketplace
- [ ] Agent gallery site
- [ ] Stripe checkout for paid agents
- [ ] Creator profiles
- [ ] Rating &amp; reviews
- [ ] 70/30 split (creator/platform)

### v0.4.0 — Cloud (optional, paid)
- [ ] Hosted agent execution
- [ ] Webhook triggers
- [ ] Cron schedules
- [ ] Team workspaces

---

## Marketplace economics (the business model)

Agentix the builder is and always will be free. The business runs on the marketplace:

```
Creator builds an agent  →  uploads to marketplace
                         ↓
Customer buys it for $39 →  pays via Stripe
                         ↓
                    Stripe takes 2.9%
                    Agentix takes 30%
                    Creator gets ~67%
```

**Why this works:**
- Zero marginal cost (no tokens to pay — buyers use their own keys)
- Network effect (more creators → more agents → more buyers → more creators)
- Aligned incentives (we only make money when creators do)

---

## Contributing

Pull requests welcome. The whole thing is one HTML file (`builder.html`) for v0.1.0 — that's intentional. Easy to read, easy to fork, easy to modify.

Open an issue if you have ideas for new node types or want to report a bug.

---

## License

MIT — do whatever you want with it.

---

## Built by

[Miguel](https://github.com/) — Mar del Plata, Argentina 🇦🇷

If you're using Agentix in production, drop me a line. I want to hear about it.
