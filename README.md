# Brief Builder

AI-powered marketing campaign brief generator for small businesses.

**Live prototype: https://claude.ai/public/artifacts/5712383a-0273-4a53-aa08-bca9d9ed69a9**

---

## What it does

Small business owners know their product but struggle to structure a marketing campaign and can't afford an agency for every brief. Brief Builder takes a guided form (business description, campaign goal, audience, channels, tone) and generates a complete, structured campaign brief — including audience analysis, key message, channel plan, content pillars, KPIs, timeline, and budget breakdown — ready to hand off to a designer or social media manager.

---

## How to use it

**Easiest — open the live prototype:**
Visit the URL above. Works in any browser, no setup, no account required.

**Locally from this repo:**
1. Clone the repo and open `campaign-brief-builder.html` directly in a browser (no server needed).
2. Fill in the form and click Generate.
3. The app calls the Anthropic API. If no API key is configured, it falls back to a template-based brief generated entirely in the browser — no network required, fully functional output.

**To enable AI generation locally:**
Expand the "Running locally or on GitHub Pages?" section below the Generate button. Paste your Anthropic API key (starts with `sk-ant-`). The key is stored in your browser's `localStorage` only — it is never sent anywhere except directly to the Anthropic API, and it is not stored in this repo.

---

## File structure

```
campaign-brief-builder.html   The entire application — one self-contained HTML file.
                               No build step, no dependencies, no framework.

README.md                      This file.

CLAUDE.md                      Project context for Claude Code sessions: architecture,
                               constraints, output schema, and key decisions.

skills/
  campaign-brief/
    SKILL.md                   Entry point for the campaign brief quality skill.
    failure-modes.md           Catalog of AI brief failure modes with before/after examples.
    prompt-patterns.md         Nine prompt techniques that address each failure mode.
    prompt-comparison.md       Side-by-side output comparison: original vs. improved prompt,
                               same fictional inputs, showing measurable differences.
```

---

## Built with Claude Code

This project was built using Claude Code. It includes a custom skill at `skills/campaign-brief/` that codifies marketing campaign brief expertise — specifically the failure modes of AI-generated briefs (generic KPIs, template budget splits, content pillars that are actually content formats, creative direction that constrains nothing) and the prompt engineering techniques that address them. The skill was used to revise the app's runtime system prompt; the before/after output is documented in `skills/campaign-brief/prompt-comparison.md`.
