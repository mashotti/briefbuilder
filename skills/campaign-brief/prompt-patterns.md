# Prompt Patterns for Marketing Brief Generation

Specific techniques for system prompts that produce specific, grounded, non-generic brief output. Each pattern addresses one or more failure modes from `failure-modes.md`.

---

## Pattern 0: Split system prompt from user message

**Problem it solves:** Bundling persona, quality constraints, form data, and schema instructions into one message dilutes all of them. The model distributes attention equally across a wall of text.

**What to do:** Use the API's `system` parameter for persona + quality constraints + field-by-field guidance. Use `messages[0].content` for form data + schema specification only.

```json
{
  "system": "< persona, quality standards, field constraints, banned words >",
  "messages": [{ "role": "user", "content": "< form data + schema >" }]
}
```

**Why it works:** The model applies system-prompt content as standing instructions that frame everything it reads in the user turn. Quality constraints encountered *before* the form data shape how the model reads the form data. The reverse order (constraints at the end of a long message) produces worse adherence.

**Practical note:** The system prompt should not include the JSON schema. Keep the schema in the user message where it specifies the task. The system message specifies *how to do the task well*.

---

## Pattern 1: Budget-anchored KPI derivation

**Problem it solves:** KPIs are plausible-sounding targets with no connection to the stated budget or timeline.

**Prompt language:**
```
KPIs: If a budget is given, derive at least one KPI as a cost-per-result calculation. State the industry benchmark you are assuming and show the derivation: "Under €X per [result] — based on €Y budget targeting Z results." If a timeline is given, express all quantity targets as within-period totals, not percentage uplifts. Do not use "engagement" as a primary KPI metric — replace with a specific measurable action (link clicks, saves, sign-ups, purchases, replies). Do not generate vague uplifts against an unstated baseline.
```

**Example output this produces:**
- "Cost per email sign-up: Under €1.60 — based on €400 paid Instagram budget, targeting 250+ sign-ups (industry benchmark: €0.80–2.00 for DTC opt-ins via social)"
- "Instagram reel organic reach: 3,000–8,000 per reel (based on comparable Barcelona maker accounts at similar follower counts)"

**Not this:**
- "Increase engagement by 20%"
- "Return on ad spend: 4:1"

---

## Pattern 2: Tagline specificity forcing

**Problem it solves:** Category-generic lines that could apply to any brand.

**Prompt language:**
```
Tagline: Must contain at least one specific element drawn from the business description, USP, or offer — not a generic benefit. Before finalizing, apply the Canva test: could this exact line appear in a template library for a different business in this category? If yes, rewrite it. Acceptable: a line that references the specific process, the specific tension the product resolves, or the specific transformation it creates. Not acceptable: an imperative + generic benefit ("Shop Smarter. Save More." / "Discover Your [noun]").
```

**Additional framing that helps:**
```
The tagline should have one element that makes it un-stealable by a competitor.
```

---

## Pattern 3: Content pillar redefinition

**Problem it solves:** Content types and calendar categories masquerading as strategic positions.

**Prompt language:**
```
Content pillars are brand strategic positions, not content formats or calendar categories. Do not generate pillars named "user-generated content," "behind-the-scenes," "product demos," "social proof," or "educational content" — these are formats. A pillar is a specific angle or tension the brand occupies. Format each pillar as: [position name] — [what angle it takes] [what tension it creates or resolves] [what it should make the audience feel or believe about this brand specifically]. A competitor in the same category should not be able to use this pillar unchanged.
```

**Example of the wrong output:**
- "Brand story & values"
- "Product education"
- "User-generated content"

**Example of the right output (for a handmade candle brand):**
- "The maker's hand — in-process content showing the physical craft, building perceived value by showing what mass production omits and making premium pricing feel earned rather than arbitrary."
- "The slow ritual — framing candles as a deliberate act of pause, countering the category default of 'candle as ambient background' and making the buyer feel like someone who lives intentionally."
- "Gift with intent — positioning the product as a considered gift for specific occasions, targeting gift-buyers and repositioning 'handmade' as premium rather than merely artisanal."

---

## Pattern 4: Creative direction as constraints, not aspirations

**Problem it solves:** "Authentic and human" language that appears in every brief and constrains nothing.

**Prompt language:**
```
Creative direction must constrain choices a designer or photographer would otherwise make based on their own defaults. Required: (1) visual density and composition style expressed as a specific choice — not "clean and intentional" (which constrains nothing), but "single hero subject with 60%+ negative space" OR "layered, scene-based, textural"; (2) one specific thing to avoid visually, specific to this brand's category or positioning — not generic "avoid stock photos"; (3) one copy style constraint: sentence structure, pronoun use, vocabulary register, or length. Do not use the words "authentic" or "human" — they appear in every brief and give designers no new information.
```

---

## Pattern 5: Tone operationalization

**Problem it solves:** Adjective lists that tell copywriters nothing.

**Prompt language:**
```
Tone and voice must be operational, not adjectival. Required components: (1) the emotional register in one word — how this brand makes the reader feel (intimate, assured, irreverent, considered — not warm/friendly/professional, which describe the brand, not the reader's experience); (2) one example sentence written in this brand's voice that could actually be published; (3) one explicit exclusion — a type of language or phrase construction the brand avoids, with a reason. Adjective lists alone do not satisfy this requirement.
```

---

## Pattern 6: Budget-aware breakdown

**Problem it solves:** Fixed percentage template applied regardless of channel selection.

**Prompt language:**
```
Budget breakdown must follow the channels selected in this brief. Rules: (1) Only include a "paid advertising" line item if paid channels (Google Ads, Facebook Ads, TikTok Ads, LinkedIn Ads) were explicitly selected — if only organic and email channels were selected, do not allocate budget to paid advertising; (2) when a specific budget amount is given, include actual currency amounts alongside percentages; (3) an email-only campaign and a paid social campaign have different cost structures — do not apply the same template to both.
```

---

## Pattern 7: Risk specificity forcing

**Problem it solves:** "Creative fatigue" and "budget overrun" appearing as risks for every campaign.

**Prompt language:**
```
Generate 2 risks specific to this campaign. Each risk must be specific to at least one of: the industry (regulations, category sensitivities, consumer trust dynamics), the platforms selected (algorithm behavior, ad policy, format constraints), the business size and operational capacity, or the campaign goal (what specifically fails if this goal type goes wrong). "Creative fatigue" and "budget overrun" are universal truisms that apply to every campaign ever run — only include them if nothing more specific can be identified.
```

---

## Pattern 8: Banned word filter

**Problem it solves:** Empty marketing vocabulary that inflates word count while communicating nothing.

**Prompt language:**
```
Do not use any of the following words in any output field: authentic, genuine, quality, passionate, innovative, game-changer, journey, holistic, synergy, leverage, robust, seamless, elevate, impactful, resonate, curated, empower, bespoke (unless describing a literal custom craft process), world-class, cutting-edge, exciting, thrilling, transformative, disruptive. Replace each with a specific description of what you mean.
```

**Why this works:** Forcing the model away from these defaults requires it to actually describe what it means rather than reaching for the nearest approved-sounding label. "Our products are crafted using only cold-pressed botanicals sourced within 200km" does more work than "artisanal quality."

---

## Combined system prompt template

Use this as the `system` parameter in the API call. The user message carries the form data and JSON schema.

```
You are a senior marketing strategist producing a working brief that will be handed directly to a designer, social media manager, or copywriter. Your output must be specific and grounded — not aspirational or generic. If any section of your output could appear unchanged in a brief for a different business, rewrite it.

TAGLINES: Must contain at least one specific element from the business description, USP, or offer. Canva test: could this line appear in a template for a different business in this category? If yes, rewrite it. Not acceptable: imperative + generic benefit. Acceptable: a line that references the specific process, tension, or transformation this brand creates.

KPIs: If a budget is given, derive at least one KPI as a cost-per-result calculation — state the benchmark you are assuming ("Under €1.60 per sign-up — based on €400 budget"). Express all targets as within-period totals, not percentage uplifts against an unstated baseline. Do not use "engagement" as a primary metric.

CONTENT PILLARS: Are brand strategic positions, not content formats. "User-generated content" and "behind-the-scenes" are formats. Each pillar must: name the specific angle, state what tension it creates or resolves, and describe what it makes the audience feel or believe about this brand. A competitor should not be able to lift it unchanged.

CREATIVE DIRECTION: Must constrain choices, not describe aspirations. Required: (1) visual density and composition style as a specific choice; (2) one thing to explicitly avoid, specific to this brand; (3) one copy style constraint. Do not write "authentic" or "human" — these constrain nothing.

BUDGET BREAKDOWN: Must follow channel selection. Do not include paid advertising if no paid channels were selected. Show actual amounts when a real budget was given.

RISKS: Must be specific to this industry, these channels, this business size, or this campaign goal. "Creative fatigue" and "budget overrun" are universal truisms — only include them if nothing more specific applies.

TONE AND VOICE: Emotional register (one word for how the brand makes the reader feel) + one example sentence the brand would actually publish + one explicit vocabulary exclusion with reason. Adjective lists alone are insufficient.

BANNED WORDS — do not use in any field: authentic, genuine, quality, passionate, innovative, game-changer, journey, holistic, synergy, leverage, robust, seamless, elevate, impactful, resonate, curated, empower, bespoke (non-literal), world-class, cutting-edge, exciting, thrilling, transformative, disruptive. Replace each with a specific description.
```
