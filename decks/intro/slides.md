---
title: Introduction to casestudy-by-sasavasic
theme: default
highlighter: shiki
lineNumbers: false
info: |
  ## casestudy-by-sasavasic
  An introduction to this multi-deck Slidev repository.
---

# casestudy-by-sasavasic

A multi-deck Slidev presentation hub hosted on GitHub Pages

<div class="pt-4 text-sm opacity-70">
  Navigate between decks at the <a href="../../">landing page</a>
</div>

---

# What is This Repository?

A showcase of **multiple Slidev decks** deployed from a single GitHub repository.

- 📂 **Organised** — each deck lives under `decks/<name>/`
- 🚀 **Automated** — GitHub Actions builds and publishes every deck
- 🤖 **AI-powered** — GitHub Copilot and Claude assist slide authoring
- 🔗 **Navigable** — a landing page links to all available decks

---

# Repository Structure

```
casestudy-by-sasavasic/
├── .github/workflows/deploy.yml  ← CI/CD pipeline
├── .vscode/extensions.json       ← Recommended extensions
├── decks/
│   ├── intro/          ← This deck
│   └── ai-skills/      ← AI skill integration deck
├── landing/index.html  ← Navigation hub
├── package.json        ← Workspace config
└── INSTRUCTIONS.md     ← Full setup guide
```

---

# Getting Started

Install dependencies and start a dev server:

```bash
git clone https://github.com/K8Cill/casestudy-by-sasavasic.git
cd casestudy-by-sasavasic
npm install
npm run dev:intro
```

Open [localhost:3030](http://localhost:3030) in your browser.

---

# Adding a New Deck

1. Create `decks/my-deck/slides.md`
2. Create `decks/my-deck/package.json` with build scripts
3. Add a card in `landing/index.html`
4. Push — the CI pipeline deploys it automatically

See **INSTRUCTIONS.md §5** for the full walkthrough.

---

# AI Skill Integration

This repo supports two AI assistants out of the box:

| Tool | Purpose |
|------|---------|
| **GitHub Copilot** | Inline completion & chat |
| **Claude (Anthropic)** | Content generation & review |

Both are recommended via `.vscode/extensions.json`.

---

# GitHub Pages Deployment

Every push to `main` triggers the workflow:

1. 📦 `npm ci` — install dependencies
2. 🔨 `slidev build` — build each deck with its `--base` path
3. 📁 Assemble `_site/` staging directory
4. 🚀 Deploy to GitHub Pages via `actions/deploy-pages`

---

# Learn More

- [Slidev Docs](https://sli.dev/guide/)
- [GitHub Copilot](https://docs.github.com/en/copilot)
- [Anthropic Claude](https://docs.anthropic.com/)
- [INSTRUCTIONS.md](../../INSTRUCTIONS.md)
