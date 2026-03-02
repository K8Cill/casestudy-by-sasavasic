# casestudy-by-sasavasic

A multi-deck [Slidev](https://sli.dev/) presentation repository with automated GitHub Pages deployment and AI skill integration (GitHub Copilot & Claude).

## Quick Start

```bash
git clone https://github.com/K8Cill/casestudy-by-sasavasic.git
cd casestudy-by-sasavasic
npm install
npm run dev:intro        # preview the intro deck
npm run dev:ai-skills    # preview the AI skills deck
```

## Available Decks

| Deck | Description |
|------|-------------|
| [intro](decks/intro/) | Repository overview and getting started guide |
| [ai-skills](decks/ai-skills/) | GitHub Copilot & Claude integration with Slidev |

## Deployment

Every push to `main` triggers a GitHub Actions workflow that builds all decks and deploys them to GitHub Pages.  Enable Pages under **Settings → Pages → Source: GitHub Actions**.

## Documentation

See [INSTRUCTIONS.md](INSTRUCTIONS.md) for the full setup guide, including how to add new decks, configure GitHub Pages, and integrate AI skills.
