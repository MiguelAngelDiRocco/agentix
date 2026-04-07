# Agentix · Example Agents

A growing collection of pre-built agents you can import into the Agentix builder. Each `.agentix.json` file is a complete agent — drop it into the builder via **↑ Import** and hit **▶ Run**.

These are the seed agents for what will eventually become the Agentix Marketplace. All of them work with both providers (Ollama local, or Anthropic API).

| Agent | What it does | Best for |
|---|---|---|
| `customer-support-classifier.agentix.json` | Classifies a support message and routes urgent ones to escalation | Support teams, e-commerce |
| `email-summarizer.agentix.json` | Turns long emails into 3 bullets + an action item | Anyone with a busy inbox |
| `lead-qualifier-b2b.agentix.json` | Scores B2B leads as HOT or COLD and routes them accordingly | Sales teams, SDRs |
| `social-post-generator.agentix.json` | Generates 5 social post variants from any topic in different tones | Creators, marketers, founders |
| `translator-es-en.agentix.json` | Auto-detecting Spanish ↔ English translator with cultural nuance | LATAM businesses, bilingual teams |
| `whatsapp-clinic-autoresponder.agentix.json` | Classifies patient WhatsApp messages and triggers urgent escalation | Health clinics in LATAM |

## How to use them

1. Open `builder.html` (locally or from the public URL)
2. Click **↑ Import** in the top right
3. Pick one of the `.json` files from this folder
4. Hit **▶ Run** — the example input is already loaded

## Want to contribute an agent?

Open a PR with your agent file in this folder. Requirements:

- File name: `kebab-case.agentix.json`
- Must include `name`, `version`, `description` fields
- Must have a working example input in the trigger node
- Test it end-to-end before submitting
- One sentence in this README's table

We'll review and merge if it works and is generally useful.

## Coming soon (the marketplace)

Once Agentix v0.2 ships with the marketplace, contributors will be able to set a price on their agents and earn a 70/30 split on sales. These open source examples will stay free forever — they're the demo, not the product.
