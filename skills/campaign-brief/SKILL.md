# Skill: Campaign Brief Quality

Use this skill when working with AI-generated marketing campaign briefs — writing or revising system prompts for brief generators, diagnosing weak output, or designing brief schemas.

Load both reference files before doing anything:
- [`failure-modes.md`](failure-modes.md) — per-section failure catalog with before/after examples
- [`prompt-patterns.md`](prompt-patterns.md) — specific prompt techniques keyed to each failure

---

## When to use this skill

- You are writing or revising a system prompt that asks a model to generate a marketing brief
- You are reviewing AI brief output and need to diagnose why it feels generic or untrustworthy
- You are building a brief schema or form and want to prevent failure modes at the structural level

---

## Quick diagnostic — run before touching anything

Flag each of these as failures requiring intervention:

**KPIs**
- [ ] Any KPI expressed as "increase X by Y%" with no stated baseline or budget connection
- [ ] "Engagement" used as a primary KPI metric (what engagement? measured how? by when?)
- [ ] No KPI is derived from the stated budget (if a budget was given)
- [ ] Targets don't scale with the timeline (4-week and 12-week campaigns have the same numbers)

**Taglines**
- [ ] Could appear in a Canva template for a different business in the same category
- [ ] Contains only a generic benefit with no element specific to this brand
- [ ] Generic imperative + noun: "Discover Your...", "Shop Smarter...", "Be the First..."

**Budget breakdown**
- [ ] Paid advertising line item appears even though no paid channels were selected
- [ ] Percentage split is identical regardless of which channels were selected
- [ ] Shows only percentages when a real budget amount was given

**Content pillars**
- [ ] Any pillar is a content format: "user-generated content", "product demos", "behind-the-scenes"
- [ ] All three pillars could be lifted unchanged by a competitor in the same industry
- [ ] Pillars function as calendar categories, not strategic positions

**Creative direction**
- [ ] Contains "authentic" or "human" with no specific constraint attached
- [ ] Gives no guidance on what NOT to include
- [ ] No copy style constraint — only visual direction
- [ ] A designer could read this and not change a single decision they would have made anyway

**Tone and voice**
- [ ] Described only as adjectives: "warm, friendly, professional, yet approachable"
- [ ] Contains no example of the voice in action
- [ ] Gives no vocabulary guidance

**Risks**
- [ ] "Creative fatigue" is listed
- [ ] "Budget overrun" is listed
- [ ] Any risk applies equally to every campaign regardless of industry, channel, or goal

**Banned words** — flag any occurrence of:
authentic, genuine, quality, passionate, innovative, game-changer, journey, holistic, synergy,
leverage, robust, seamless, elevate, impactful, resonate, curated, empower, bespoke (non-literal),
world-class, cutting-edge, exciting, thrilling, transformative, disruptive

---

## When revising a system prompt

1. Run the diagnostic above against current output (use a real test case — handmade product, local service — not a generic B2B scenario, which masks most of these failures)
2. Note which failure modes are present; each maps to a specific pattern in `prompt-patterns.md`
3. Split the prompt: move persona + quality constraints to the API `system` parameter, keep form data + schema in the user message
4. Apply the relevant patterns; the most impactful are: budget-anchored KPIs, pillar redefinition, creative direction constraints, risk specificity forcing
5. Re-test with the same scenario; check specifically that: tagline contains something from the business description, KPI targets are derived from the budget, no banned words appear, budget breakdown reflects the selected channels

---

## What this skill does NOT cover

- Ad copy or social media caption writing (different failure modes)
- Campaign strategy (channel selection, audience segmentation)
- Brand identity or visual design
- Anything requiring real market data — this skill addresses structural prompt failures, not market research gaps
