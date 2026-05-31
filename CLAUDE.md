# Brief Builder — Claude Code context

## What this is

A single-page web app that helps small business owners create structured marketing campaign briefs. The user fills in a guided form (business, campaign goal, audience, channels, tone), clicks Generate, and gets a professional, shareable brief they can hand to a designer or social media manager.

**Problem it solves:** Small business owners know their product but can't structure marketing or afford an agency for every campaign.

## Tech stack

- Single HTML file (`campaign-brief-builder.html`). No build step, no dependencies, no framework.
- Google Fonts: DM Serif Display (headings) + DM Sans (body), loaded via CDN.
- Anthropic API (`/v1/messages`, model `claude-sonnet-4-20250514`) called client-side.
- Browser localStorage for API key persistence.

## Architecture

Everything is in one file in this order: `<style>` → `<body>` HTML → `<script>`.

The right panel has three states toggled by `setUI(state)`:
- `empty` — initial prompt
- `loading` — spinner while API call runs
- `output` — the rendered brief (via `renderBrief()`)

**Two brief generation paths:**
1. **AI path** — `fetch` to Anthropic API. Works natively in Claude.ai artifact runtime (runtime proxies auth). Works in any browser if user has saved an Anthropic API key via the collapsible "API key" section (key stored in `localStorage` under `bb_api_key`).
2. **Template path** — `generateTemplateBrief(formData)` runs if the API call fails for any reason (no key, CORS, network down, timeout). Produces a realistic structured brief from the form data using goal-specific content pools. Renders through the same `renderBrief()` function so copy/print/export all work. Shows a blue info banner explaining it's a template brief.

This means **the app never lands in a broken or error-only state** in any environment.

## How to run / test

- **Claude.ai artifact runtime**: open the file as an artifact. API calls work automatically.
- **Local (file://)**: double-click the HTML file to open in browser. On first use, expand "Running locally or on GitHub Pages?" to save an Anthropic API key. Without a key, template mode activates automatically.
- **GitHub Pages**: commit the file to a `gh-pages` branch or `docs/` folder and enable Pages in repo settings. Same behaviour as file://.

No server needed. No `npm install`.

## Hard constraints

1. **Single HTML file.** Do not split into multiple files, add a bundler, or add npm dependencies.
2. **No API keys in committed files.** Keys are entered by the user and stored in `localStorage` only.
3. **Preserve the design.** Typography: DM Serif Display + DM Sans. Colors: defined as CSS custom properties in `:root`. Layout: sticky nav + two-column grid (420px form panel + fluid brief panel).
4. **Preserve all user-visible functionality:** form intake, brief output, Copy, Print/Export buttons.

## Output JSON schema

The API prompt requests — and `generateTemplateBrief()` returns — this exact shape:

```json
{
  "campaignName": "string",
  "tagline": "string",
  "executiveSummary": "string",
  "objective": "string",
  "secondaryObjective": "string | null",
  "targetAudience": {
    "description": "string",
    "demographics": "string",
    "psychographics": "string",
    "painPoints": "string"
  },
  "keyMessage": "string",
  "valueProposition": "string",
  "toneAndVoice": "string",
  "channels": [{ "name": "string", "purpose": "string", "contentType": "string" }],
  "contentPillars": ["string"],
  "kpis": [{ "metric": "string", "target": "string" }],
  "timeline": [{ "week": "string", "phase": "string", "activities": "string" }],
  "budget": {
    "total": "string",
    "breakdown": [{ "item": "string", "amount": "string", "percent": "string" }]
  },
  "callToAction": "string",
  "creativeDirection": "string",
  "successCriteria": "string",
  "risks": "string"
}
```

`renderBrief(b, bizName, campName, isAI)` consumes this shape directly. All fields are optional-safe (`?.` and `|| []` guards throughout).

## Campaign brief quality skill

A custom skill lives at `skills/campaign-brief/`. Load it when working on the runtime system prompt or reviewing generated brief output.

**Files:**
- `SKILL.md` — entry point: diagnostic checklist, when to use, how to apply
- `failure-modes.md` — per-section failure catalog with before/after examples (taglines, KPIs, content pillars, creative direction, tone, risks, psychographics, budget breakdown, executive summary)
- `prompt-patterns.md` — nine specific prompt techniques keyed to each failure mode, plus a combined system prompt template

**Relationship to the runtime prompt:**
The `systemPrompt` constant in `generateBrief()` (HTML line ~781) is the output of applying this skill. It uses Pattern 0 (system/user split), Pattern 1 (budget-anchored KPIs), Pattern 3 (pillar redefinition), Pattern 4 (creative direction constraints), Pattern 5 (tone operationalization), Pattern 6 (budget-aware breakdown), Pattern 7 (risk specificity), and Pattern 8 (banned word filter) from `prompt-patterns.md`.

The JSON schema field descriptions in the `prompt` constant were changed from type annotations to specific guidance — this is the other lever the skill identified: the schema spec is also a prompt.

**To use in a future session:** Read `SKILL.md`, `failure-modes.md`, and `prompt-patterns.md` as context before revising the system prompt or evaluating brief output.

## Key decisions made during the standalone fix

**Why template fallback instead of just showing an error:** The requirement is that every environment must render something coherent. A template brief is fully usable — same structure, same copy/print/export — and it's honest about its source via the info banner.

**Why `localStorage` for the API key:** No secrets can be committed to the repo. The user enters their own key, it never leaves their browser. The collapsible `<details>` section keeps the UI clean for users who don't need it (Claude.ai users).

**Why `anthropic-dangerous-direct-browser-access: true`:** Required by Anthropic's API for direct browser-to-API calls when a user-supplied key is present. Omitting it causes CORS errors even with a valid key.

**Why `max_tokens: 2000`:** The original value of 1000 was too low for the full JSON schema, causing truncated/invalid JSON and silent failures. 2000 gives enough headroom.

**Why 30s timeout with `AbortController`:** On a device with no network, `fetch` hangs indefinitely. The abort ensures the fallback triggers in reasonable time rather than spinning forever.

**Why `anthropic-version: 2023-06-01` is always sent:** It's a required header per Anthropic API docs. The Claude.ai runtime ignores extra headers gracefully, but adding it means the request is valid if the runtime ever stops proxying.
