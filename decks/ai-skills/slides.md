---
title: AI Skill Integration with Slidev
theme: default
highlighter: shiki
lineNumbers: false
info: |
  ## AI Skill Integration
  How to use GitHub Copilot and Claude with Slidev.
---

# AI Skill Integration

Using **GitHub Copilot** and **Claude** with Slidev

<div class="pt-4 text-sm opacity-70">
  <a href="../../">← Back to all decks</a>
</div>

---

# Why AI Skills in Slide Authoring?

AI assistants accelerate deck creation by:

- ✍️ **Drafting** slide content from bullet points or outlines
- 🔍 **Reviewing** structure, clarity, and tone
- 🖼️ **Generating** diagram ideas and code examples
- 🌐 **Translating** or localising existing decks

---

# GitHub Copilot

GitHub Copilot is an AI pair programmer embedded in VS Code.

**Setup:**
1. Install the `GitHub.copilot` extension
2. Sign in with your GitHub account
3. Enable Copilot in VS Code settings

**Useful in Slidev:**
- Autocompletes Markdown and YAML front-matter
- Suggests code examples on code slides
- Use `Ctrl+I` for Copilot Chat in any file

---

# Copilot Chat Example

Open Copilot Chat (`Ctrl+I`) and try:

```
@workspace Create a slide comparing REST vs GraphQL APIs
with a markdown table and a code example for each.
```

Copilot generates the slide content directly in your editor.

---

# Claude (Anthropic)

Claude is a large language model by Anthropic with strong
writing and reasoning capabilities.

**VS Code Extension:**
```
Extension ID: Anthropic.claude-code
```

**Environment variable required:**
```bash
export ANTHROPIC_API_KEY="sk-ant-..."
```

---

# Claude API Integration

Add the SDK to a deck's dependencies:

```bash
npm install @anthropic-ai/sdk --save-dev \
  --workspace=decks/ai-skills
```

Simple usage in a Node.js script:

```js
import Anthropic from "@anthropic-ai/sdk";

const client = new Anthropic();
const msg = await client.messages.create({
  model: "claude-opus-4-5",
  max_tokens: 512,
  messages: [{
    role: "user",
    content: "Write a 3-slide summary of REST APIs."
  }]
});
console.log(msg.content[0].text);
```

---

# Security Best Practices

Never commit API keys to source control.

```bash
# .env (excluded from git via .gitignore)
ANTHROPIC_API_KEY=sk-ant-...
```

For CI/CD, store secrets in GitHub Actions:

1. **Settings → Secrets → Actions**
2. Add `ANTHROPIC_API_KEY`
3. Reference in workflow: `${{ secrets.ANTHROPIC_API_KEY }}`

---

# Recommended VS Code Extensions

| Extension | ID |
|-----------|-----|
| GitHub Copilot | `GitHub.copilot` |
| GitHub Copilot Chat | `GitHub.copilot-chat` |
| Claude | `Anthropic.claude-code` |
| Slidev | `antfu.slidev` |

All listed in `.vscode/extensions.json` — VS Code will
prompt you to install them automatically.

---

# Summary

- 🤖 **Copilot** — inline AI completion for Markdown & code
- 🧠 **Claude** — conversational AI for content generation
- 🔒 **Security** — keep API keys out of source control
- 📦 **Extensions** — pre-configured in `.vscode/extensions.json`

See **INSTRUCTIONS.md §7** for the full integration guide.
