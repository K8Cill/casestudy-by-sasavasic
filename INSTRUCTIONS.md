# casestudy-by-sasavasic — Setup & Usage Instructions

## Table of Contents

1. [Project Goals](#1-project-goals)
2. [Repository Structure](#2-repository-structure)
3. [Prerequisites & Tools](#3-prerequisites--tools)
4. [Local Development Setup](#4-local-development-setup)
5. [Adding a New Deck](#5-adding-a-new-deck)
6. [GitHub Pages Deployment](#6-github-pages-deployment)
7. [AI Skill Integration (Copilot & Claude)](#7-ai-skill-integration-copilot--claude)
8. [Navigation Landing Page](#8-navigation-landing-page)
9. [References](#9-references)

---

## 1. Project Goals

This repository hosts **multiple [Slidev](https://sli.dev/) presentation decks** from a single GitHub repository, each automatically published to **GitHub Pages** via a CI/CD workflow.  It also demonstrates how to integrate AI coding assistants—**GitHub Copilot** and **Claude (Anthropic)**—into the authoring workflow.

Key objectives:

- Maintain independent Slidev decks under `decks/<deck-name>/`.
- Deploy every deck to a dedicated sub-path on GitHub Pages (e.g. `/<repo>/decks/intro/`).
- Serve a navigation landing page at the repository root (`/<repo>/`).
- Provide first-class AI skill support for slide authoring via VS Code extensions.

---

## 2. Repository Structure

```
casestudy-by-sasavasic/
├── .github/
│   └── workflows/
│       └── deploy.yml          # CI: build all decks & deploy to GitHub Pages
├── .vscode/
│   └── extensions.json         # Recommended VS Code extensions (Copilot, Claude)
├── decks/
│   ├── intro/                  # Example deck — project introduction
│   │   ├── slides.md           # Slidev source
│   │   └── package.json        # Per-deck dependencies & build scripts
│   └── ai-skills/              # Example deck — AI skill integration overview
│       ├── slides.md
│       └── package.json
├── landing/
│   └── index.html              # Navigation hub (auto-generated links to all decks)
├── package.json                # Root workspace config
├── INSTRUCTIONS.md             # This file
└── README.md
```

---

## 3. Prerequisites & Tools

| Tool | Version | Notes |
|------|---------|-------|
| [Node.js](https://nodejs.org/) | ≥ 18 | LTS recommended |
| [npm](https://www.npmjs.com/) | ≥ 9 | Bundled with Node.js |
| [VS Code](https://code.visualstudio.com/) | latest | Primary IDE |
| GitHub Copilot | latest | VS Code extension — `GitHub.copilot` |
| Claude for VS Code | latest | VS Code extension — `Anthropic.claude-code` |

> **Optional:** [pnpm](https://pnpm.io/) or [Yarn](https://yarnpkg.com/) can replace npm as the workspace manager.

---

## 4. Local Development Setup

### 4.1 Clone the repository

```bash
git clone https://github.com/K8Cill/casestudy-by-sasavasic.git
cd casestudy-by-sasavasic
```

### 4.2 Install root dependencies

```bash
npm install
```

This installs shared tooling and triggers `npm install` in every deck workspace.

### 4.3 Start a deck in dev mode

```bash
# From the repo root:
npm run dev --workspace=decks/intro

# Or navigate to the deck directory:
cd decks/intro
npm run dev
```

Slidev opens a live-reloading preview at `http://localhost:3030` by default.

### 4.4 Build a single deck

```bash
npm run build --workspace=decks/intro
```

The static output is written to `decks/intro/dist/`.

### 4.5 Build all decks at once

```bash
npm run build:all
```

This iterates over every workspace under `decks/` and runs its `build` script.

---

## 5. Adding a New Deck

1. **Create the deck directory and source file:**

   ```bash
   mkdir -p decks/my-new-deck
   ```

2. **Create `decks/my-new-deck/slides.md`** using any Slidev front-matter:

   ```markdown
   ---
   title: My New Deck
   theme: default
   ---

   # My New Deck

   Hello Slidev!
   ```

3. **Create `decks/my-new-deck/package.json`:**

   ```json
   {
     "name": "deck-my-new-deck",
     "private": true,
     "scripts": {
       "dev":   "slidev slides.md",
       "build": "slidev build slides.md --base /casestudy-by-sasavasic/decks/my-new-deck/ --out dist"
     },
     "dependencies": {
       "@slidev/cli": "^0.50.0",
       "@slidev/theme-default": "latest"
     }
   }
   ```

   > Adjust `--base` to match `/<repo-name>/decks/<deck-name>/`.

4. **Register the deck in the landing page** (`landing/index.html`) by adding a link card:

   ```html
   <a class="card" href="decks/my-new-deck/">My New Deck</a>
   ```

5. **Commit and push.** The `deploy.yml` workflow will automatically build and publish the new deck.

---

## 6. GitHub Pages Deployment

Deployment is handled by `.github/workflows/deploy.yml` which:

1. Checks out the repository.
2. Sets up Node.js.
3. Installs dependencies (`npm ci`).
4. Builds each deck with `slidev build … --base /<repo>/decks/<name>/ --out dist`.
5. Copies all `dist/` folders and the `landing/` directory into a staging directory.
6. Publishes the staging directory to the `gh-pages` branch using the official [`actions/deploy-pages`](https://github.com/actions/deploy-pages) action.

### Enabling GitHub Pages

1. Go to **Settings → Pages** in your GitHub repository.
2. Set **Source** to **GitHub Actions**.
3. Push to `main`; the workflow triggers automatically.

### Base URL Configuration

Each deck's `package.json` build script must include a `--base` flag matching its deployment path:

```
--base /<repo-name>/decks/<deck-name>/
```

For this repository the repo name is `casestudy-by-sasavasic`.

---

## 7. AI Skill Integration (Copilot & Claude)

### 7.1 GitHub Copilot

GitHub Copilot provides AI-powered code and prose completion inside VS Code.

**Installation:**

1. Open VS Code.
2. Install the **GitHub Copilot** extension (`GitHub.copilot`).
3. Sign in with your GitHub account (requires an active Copilot subscription or free tier).

**Usage in Slidev decks:**

- Copilot autocompletes Markdown, Vue components, and front-matter YAML as you type.
- Use `Ctrl+I` (or `Cmd+I` on macOS) to open **Copilot Chat** and ask questions like:  
  `"Summarise this slide deck"` or `"Add a diagram slide comparing X and Y"`.
- Use inline suggestions (`Tab` to accept) when writing slide content.

**Copilot in this repo:**  
The `.vscode/extensions.json` file lists `GitHub.copilot` as a recommended extension, so VS Code will prompt team members to install it automatically when they open the workspace.

### 7.2 Claude (Anthropic)

Claude can be accessed directly inside VS Code via the **Claude** extension or by calling the Anthropic API from build scripts and custom Slidev plugins.

**VS Code Extension:**

1. Install **Claude** from the VS Code Marketplace (`Anthropic.claude-code`).
2. Authenticate with your Anthropic API key (set `ANTHROPIC_API_KEY` in your shell environment or VS Code settings).
3. Use the Claude panel to generate, review, or summarise slide content.

**API-based integration (optional):**

Add the Anthropic SDK to a deck's dependencies and call it from a Node.js script or Slidev addon:

```bash
npm install @anthropic-ai/sdk --save-dev --workspace=decks/ai-skills
```

Example helper script (`decks/ai-skills/scripts/generate-summary.mjs`):

```js
import Anthropic from "@anthropic-ai/sdk";

const client = new Anthropic(); // reads ANTHROPIC_API_KEY from env

const message = await client.messages.create({
  model: "claude-opus-4-5",
  max_tokens: 1024,
  messages: [
    {
      role: "user",
      content: "Summarise this slide deck in 3 bullet points: ...",
    },
  ],
});

console.log(message.content[0].text);
```

> **Security note:** Never commit your API key. Store it in a `.env` file (excluded from git via `.gitignore`) or as a GitHub Actions secret named `ANTHROPIC_API_KEY`.

### 7.3 Recommended VS Code Extensions

The `.vscode/extensions.json` file in this repository recommends:

| Extension ID | Purpose |
|---|---|
| `GitHub.copilot` | AI pair programmer |
| `GitHub.copilot-chat` | Copilot Chat panel |
| `Anthropic.claude-code` | Claude AI assistant |
| `antfu.slidev` | Slidev language support & preview |

---

## 8. Navigation Landing Page

The `landing/index.html` file provides a simple navigation hub listing all available decks.  It is deployed to the root of the GitHub Pages site.

To add a new deck to the landing page, insert a new `<a class="card">` element in the `#decks` grid:

```html
<a class="card" href="decks/my-new-deck/">
  <h2>My New Deck</h2>
  <p>Short description here.</p>
</a>
```

The landing page is intentionally dependency-free (plain HTML/CSS) to minimise build complexity and load time.

---

## 9. References

- [Slidev Documentation](https://sli.dev/guide/)
- [Slidev — Hosting on GitHub Pages](https://sli.dev/guide/hosting#github-pages)
- [Slidev — Themes](https://sli.dev/themes/gallery)
- [Slidev — Components & Layouts](https://sli.dev/builtin/layouts)
- [GitHub Copilot Documentation](https://docs.github.com/en/copilot)
- [Anthropic Claude API Documentation](https://docs.anthropic.com/)
- [GitHub Actions — Deploy Pages](https://github.com/actions/deploy-pages)
- [actions/configure-pages](https://github.com/actions/configure-pages)
- [actions/upload-pages-artifact](https://github.com/actions/upload-pages-artifact)
