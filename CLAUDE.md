# Agentix — Context for Claude Code

> This file is read automatically by Claude Code when working in this repo.
> It provides project context, conventions, and conventions so you don't have to re-explain things every session.

## What this project is

Agentix is an open source visual builder for AI agents. It's a single-file web app that lets users drag and drop nodes to build pipelines that call LLMs (Ollama locally or the Anthropic API). Runs entirely in the browser with no backend.

**Live site:** https://miguelangeldirocco.github.io/agentix/
**GitHub:** https://github.com/MiguelAngelDiRocco/agentix
**Current version:** v1.1.0
**Maintainer:** Miguel (Mar del Plata, Argentina)

## Repo structure

```
agentix/
├── builder.html         # The product. Single-file HTML+CSS+JS app. THE main file.
├── index.html           # Marketing landing page
├── README.md            # GitHub README
├── PLAN.md              # Strategic 4-week launch plan
├── CLAUDE.md            # This file
└── agents/              # Pre-built agent templates (importable .json files)
    ├── README.md
    ├── customer-support-classifier.agentix.json
    ├── email-summarizer.agentix.json
    ├── lead-qualifier-b2b.agentix.json
    ├── social-post-generator.agentix.json
    ├── translator-es-en.agentix.json
    └── whatsapp-clinic-autoresponder.agentix.json
```

## Architecture rules (IMPORTANT — do not violate without asking)

1. **`builder.html` is a SINGLE FILE.** All HTML, CSS, and JS live in one file. Do not split into separate `.css` or `.js` files. This is intentional for portability and simplicity.
2. **No build step.** No webpack, vite, npm install, package.json, transpilation, anything. The user opens the file in a browser and it works. If you're tempted to add a build step, stop and ask first.
3. **No backend.** Agentix runs 100% in the browser. No Node servers, no Python backends, no Vercel functions. Storage is `localStorage`. LLM calls go directly from the browser to Ollama (`localhost:11434`) or the Anthropic API (`api.anthropic.com`).
4. **Vanilla JS only.** No React, no Vue, no Svelte, no jQuery. Vanilla DOM manipulation. The whole point is zero dependencies.
5. **No external CSS frameworks.** No Tailwind, no Bootstrap. Custom CSS with CSS variables for theming.
6. **Two external resources allowed:** Google Fonts (`JetBrains Mono` and `Instrument Serif`). That's it.
7. **Backwards compatibility for agent files.** When you change the data model for nodes/edges, always normalize old `.agentix.json` files on import so existing agents in the `agents/` folder still load.

## Conventions

### Code style in builder.html
- Compact but readable. Single-line CSS rules where it makes sense to save lines.
- JS uses modern features (async/await, optional chaining, nullish coalescing, `replaceAll`).
- Functions are at the top level of the script, not inside an IIFE or module.
- DOM IDs are camelCase (`runBtn`, `agentName`).
- CSS classes are kebab-case (`node-header`, `palette-item`).

### Node types
Nodes are defined in the `NODE_TYPES` const at the top of the script. Each has:
- `name`, `desc`, `color`, `icon`
- `inputs` (number, usually 0 or 1)
- `outputs` (number, or `'dynamic'` for switch which uses `node.branches.length`)
- `fields` array describing the inspector form

When adding a new node type, follow this exact pattern.

### Agent file format (v1.1)
```json
{
  "name": "agent-name",
  "version": "1.1.0",
  "description": "...",
  "nodes": {
    "n1": { "id": "n1", "type": "trigger", "x": 60, "y": 200, "config": {...}, "branches": [...] }
  },
  "edges": [
    { "from": "n1", "fromPort": 0, "to": "n2", "toPort": 0 }
  ],
  "view": { "panX": 0, "panY": 0, "zoom": 1 }
}
```
The `branches` field is only present on switch nodes.
The `view` and `version` fields are optional for backwards compat.

### Commit messages
Conventional Commits style:
- `feat: add minimap viewport drag` (new feature)
- `fix: minimap rectangle desync after import` (bug fix)
- `docs: update README install steps` (docs only)
- `refactor: extract render logic into helper` (code change without behavior change)
- `style: align node header padding` (visual/CSS only)
- `chore: update agent example formatting` (housekeeping)

Keep the subject line under 72 characters and in imperative mood ("add" not "added").

## Testing changes

There is no automated test suite (yet). To test changes to builder.html:

1. Start a local server in the repo: `python3 -m http.server 8000`
2. Open `http://localhost:8000/builder.html` in the browser
3. Test the affected feature manually
4. Test that the example agents still import and run correctly. Especially:
   - `agents/customer-support-classifier.agentix.json` (basic flow with Condition)
   - `agents/whatsapp-clinic-autoresponder.agentix.json` (uses Switch with 4 branches — the heaviest test)

For LLM-related changes, test with **both providers**: Ollama (free, local — Miguel uses `llama3.2:3b` on his M4) and Anthropic API. Most of the time Ollama is enough.

## Current roadmap (4-week plan)

We're following a 4-week plan to ship v1.0 of Agentix. Status:

- **Week 1 — Motor** ✅ DONE. Switch node, variables ({{n1}} {{n2}}), multi-port edges, pan/zoom, auto-save.
- **Week 2 — UX Polish** ✅ DONE. Undo/redo, duplicate (Cmd+D), delete keyboard, command palette (Cmd+K), toasts, multi-select, minimap, polished inspector.
- **Week 3 — Content** ⏳ NEXT. 15 quality agent templates, screenshots, 60s video demo, redesigned landing, LICENSE, CONTRIBUTING.md, issue templates.
- **Week 4 — Launch** ⏳ Beta with 5 friends, fixes, then coordinated launch on Show HN, Reddit (r/LocalLLaMA, r/SideProject), Product Hunt, LinkedIn, Twitter.

The full plan is in `PLAN.md`.

## Known limitations (deferred to v2)

- No tool use / function calling — the LLM only processes text in pre-defined steps, it doesn't decide which node to run next. (This is the biggest gap vs. "real" agents.)
- No webhook trigger — agents only run when the user clicks Run.
- No scheduler / cron — same reason.
- No memory / conversation state across runs.
- No real loop node (no iterating over arrays).
- No file/binary support — only text passes between nodes.

If you're tempted to add any of these, ask first. They're all on the v2 list and need design discussion.

## What "working" looks like

When in doubt about whether a change is good:

1. The builder still opens in the browser with no errors in the console.
2. All 6 example agents still import and run.
3. The new feature has clear UX (no hidden interactions, clear visual feedback).
4. The single-file constraint is preserved.
5. The HTML+JS still parses cleanly.

## Working with Miguel

- Communication: Spanish (Argentine/rioplatense). Direct, practical, no fluff.
- Background: he's learning data science but reads JS and can run terminal commands. Strong on product instinct, weaker on raw coding.
- Hardware: MacBook Air M4 16GB. Ollama running locally with `llama3.2:3b`.
- He values: working code over clever abstractions, honest tradeoffs over hype, shipping over perfecting.
- Don't suggest: heavy frameworks, build pipelines, paid services (he's keeping costs at $0), or anything that violates the architecture rules above.
