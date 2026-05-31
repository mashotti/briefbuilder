# Campaign Brief Failure Modes

Per-section catalog of how AI models fail at generating credible marketing briefs. Each entry names the pattern, explains why the model produces it, and shows what the output should look like instead.

---

## Taglines

**The failure pattern:** Generic benefit + imperative verb. The model generates lines that could apply to any brand in any category: "Quality You Can Count On," "Shop Smarter, Save More," "Be the First to Know," "Discover Your [noun]."

**Why it happens:** The model pattern-matches to the statistical average of advertising language in its training data. Without a specificity constraint, it generates what a tagline *sounds like*, not what this brand's tagline *should be*.

**The test:** Read the tagline and ask: could this exact line appear in a Canva template library for a different business in the same category? If yes, it failed.

**What good looks like:** The tagline earns its specificity by containing at least one element that proves it was written for this brand — a reference to the process, a tension the product resolves, the specific transformation it creates, or an unexpected angle on the category.

**Before (handmade soy candle brand):** `"Quality You Can Count On"`

**After:** `"Slow burn. Worth the wait."` — references the literal product property and positions the brand against fast/mass production without saying either of those words.

**Before (organic cleaning products):** `"The Natural Choice for Your Home"`

**After:** `"Clean enough to eat off. Kind enough not to."` — surfaces the specific tension between "effective cleaning" and "non-toxic formula" that is this product's actual value claim.

---

## Executive Summary

**The failure pattern:** Re-stating the form inputs in paragraph form. "This campaign aims to drive sales for [Business Name] by leveraging [channel list] to reach [audience description from the form]."

**Why it happens:** The model summarizes what it was given rather than synthesizing why this approach is strategically sound. Summarizing is easier than reasoning.

**The test:** Remove all proper nouns. Does the summary still contain something specific to this situation, or does it read as boilerplate that could describe any campaign?

**What good looks like:** The executive summary should contain at least one thing that wasn't explicitly stated in the form — the strategic logic, a key insight about the audience's psychology, or the specific opportunity this campaign is designed to exploit.

**Before (same candle brand):** "This campaign aims to drive sales for Sol & Wax Candles by leveraging Instagram and email to reach women aged 25–40 interested in home décor and sustainability."

**After:** "Sol & Wax sits at the intersection of two purchase motivations — the gifting occasion and the 'slow home' aesthetic — that have driven the handmade candle category's growth. This campaign converts that cultural tailwind into purchase intent by leading with the maker's story (gifting occasions need the narrative) rather than product specifications, while using a limited-run launch offer to overcome the 'I'll treat myself later' deferral pattern typical in this segment."

---

## KPIs & Success Metrics

**The failure pattern:** Vague directional targets expressed as percentage uplifts against an unstated baseline: "Increase engagement by 20%," "Grow social media presence," "4:1 ROAS" (where did 4:1 come from?).

**Why it happens:** The model doesn't know the baseline, so it defaults to relative terms. Percentage uplifts sound credible without committing to anything verifiable. The model has also absorbed "4:1 ROAS" and "35% open rate" as common benchmark numbers and repeats them regardless of context.

**The specific failures:**
- "Engagement" as a KPI metric (which platform? what counts as engagement? saves, comments, shares, clicks?)
- Targets identical across a 4-week campaign and a 12-week campaign
- No KPI derived from the actual budget given (if the user entered €800, at least one KPI should be calculated from it)
- ROAS targets for a brand that selected no paid channels

**What good looks like:** Every KPI should pass: "Can I tell unambiguously, at the end of this campaign, whether this was achieved?" When a budget is given, derive at least one cost-per-result target explicitly.

**Calculation pattern:** Budget ÷ cost-per-result benchmark = target quantity. Example: €800 budget, goal is leads, industry CPL for social-driven SMB leads = €8–15 → target: 55–100 leads over the campaign period. State the benchmark.

**Before:** `{ "metric": "Return on ad spend (ROAS)", "target": "4:1" }` (arbitrary, budget-free)

**After:** `{ "metric": "Cost per email sign-up (paid Instagram → landing page)", "target": "Under €1.60 — based on €400 paid budget, targeting 250+ sign-ups" }`

**Before:** `{ "metric": "Social reach", "target": "100K+ impressions" }` (for a local brand, €800 budget)

**After:** `{ "metric": "Instagram reel organic reach", "target": "3,000–8,000 per reel across campaign period (baseline: comparable Barcelona maker accounts at launch)" }`

---

## Budget Breakdown

**The failure pattern:** Fixed template percentages applied regardless of channel selection: "40% paid advertising, 25% content creation, 15% email tools, 10% influencer, 10% contingency" — for every campaign, every time.

**Why it happens:** The model applies the statistically average marketing budget allocation. It doesn't cross-reference which channels were actually selected.

**The specific problem:** A user selects only "Instagram" and "Email newsletter" (no paid channels). The output allocates 40% of their €800 to "paid social & search advertising" — €320 for channels they didn't ask for. This is actively misleading advice that will waste their money.

**What good looks like:** The breakdown follows from the channel selection. No paid advertising line item unless paid channels were explicitly chosen. When a real budget is given, show actual amounts, not only percentages. The split for an email-only campaign looks nothing like the split for a paid social campaign.

**Before (email + organic Instagram only, €800):**
- Paid social & search advertising: 40% — €320
- Content creation & design: 25% — €200
- Email tools & automation: 15% — €120
- Influencer & partnerships: 10% — €80
- Contingency: 10% — €80

**After (same channel selection):**
- Content creation & photography: 50% — €400 (4–6 product shots or lifestyle shoots)
- Email platform & list growth tools: 15% — €120 (platform subscription + sign-up tool)
- Design assets & templates: 20% — €160 (Canva Pro, story templates, grid aesthetic)
- Contingency / ad-hoc boosts on top-performing posts: 15% — €120

---

## Content Pillars

**The failure pattern:** Content *types* presented as strategic *positions*: "Brand story & values," "Product benefits & comparisons," "Customer reviews & social proof," "Behind-the-scenes content," "User-generated content."

**Why it happens:** "Content pillars" is an industry term the model associates with "content calendar categories." Marketing blog posts constantly use these examples. The model generates what content pillars are *called*, not what they *do*.

**The specific problem:** These three pillars — brand story, product education, social proof — could be the content strategy for literally any product brand. They constrain nothing about what angle to take, what the brand stands for, or why this content would be different from a competitor's.

**What good looks like:** Pillars are strategic positions — the specific angles or tensions this brand occupies in its category. Each pillar should function as a brief for a content creator: what the content should make the audience feel or believe about this brand, not just what subject matter it covers. A competitor in the same category should not be able to use your pillar unchanged.

**Before (botanical candle brand):**
- "Brand story & values"
- "Product benefits & comparisons"
- "Customer reviews & social proof"

**After (same brand):**
- "The maker's hand — in-process content showing the physical craft: pouring, wick placement, botanical sourcing. Builds perceived value by showing what mass production omits and making premium pricing feel earned."
- "The slow ritual — candles framed as a deliberate act of pause, not ambient decoration. Counters the category default of 'candle as background.' Makes the buyer feel like someone who lives intentionally."
- "Gift with intent — scenarios positioning the candle as a considered gift, not a fallback. Targets gift-buyers and repositions 'handmade' as premium rather than merely artisanal."

---

## Creative Direction

**The failure pattern:** "Authentic, human, real photography, clean and intentional, consistent with brand identity."

**Why it happens:** These phrases represent the current consensus advice on brand content. The model has absorbed them from thousands of marketing briefs and best-practice articles. They are the average of what creative direction says, not a specific direction for this brand.

**The specific problem:** This text appears in essentially every AI brief. "Authentic" is not a creative constraint — it's an aspiration that every designer already shares. It cannot tell a designer or photographer how to make a single decision differently.

**What creative direction is actually for:** Constraining the choices a designer or photographer would otherwise make based on their own defaults. It's only useful if it could cause them to do something different from what they'd do without it.

**Required elements:**
1. Visual density and composition — expressed as a choice, not an aspiration ("single hero subject, strong negative space" OR "layered, scene-based, textural" — these are mutually exclusive and both plausible defaults)
2. One specific thing to avoid — not "stock photography" (everyone knows this), but something specific to this brand's category or positioning
3. One copy style constraint — sentence structure, pronoun stance, vocabulary register

**Before:** "Visuals should feel warm, human, and authentic — real photography over stock, clean and intentional with consistent brand identity."

**After:** "Single-product compositions in warm natural light with 60%+ negative space — this brand sells feel over specification, so the shot should do the explaining. Avoid cluttered flat-lays and white-backdrop product photography; that's catalogue language, not story language. Copy: short fragments in second person ('Your Sunday ritual.' 'Light it. Mean it.') — no full sentences explaining the product; the image carries meaning and the words frame it."

---

## Tone & Voice

**The failure pattern:** Adjective lists that could describe any premium consumer brand: "Warm, authentic, professional, yet approachable."

**Why it happens:** These are the descriptors marketing departments use to define their tone. The model generates what tone descriptions *look like*, not what this brand's tone *sounds like*.

**The specific problem:** A copywriter handed "warm and friendly" has no new information. They could write almost anything and argue it fits. The description cannot fail, so it cannot succeed at anything specific. It functions purely as a category signal ("this is a nice friendly brand") with zero operational value.

**What good looks like:** Tone guidance should make certain phrases obviously in-brand and others obviously out-of-brand. A copywriter should be able to look at a sentence and tell you whether it passes.

**Required elements:**
1. Emotional register — one word that describes the feeling the brand creates (intimate, assured, irreverent, considered — not warm/friendly/professional, which are all adjectives for the brand, not feelings for the reader)
2. One example sentence the brand would actually publish
3. One explicit exclusion — a type of language or construction the brand avoids, with a reason

**Before:** "Warm, authentic, and confident — speaking directly to customer needs without jargon."

**After:** "Intimate and considered. Speaks like someone who made this for you specifically, not at a market. Example: 'Each batch is poured in small runs — not for the label, but because that's the only way we can check every single one.' Avoids superlatives and 'excited to announce' constructions. Never uses third-person brand voice; always first-person plural ('we made') or direct second person ('you'll notice')."

---

## Risks

**The failure pattern:** "Creative fatigue: vary content formats and cap frequency. Budget overrun: set daily caps and review weekly."

**Why it happens:** These are the two most common marketing risk categories in the model's training data. They are correct at a category level. They are also true of every campaign that has ever run. They provide no information about this situation.

**What real risk analysis looks like:** Each risk should be specific to at least one of:
- **The industry** — regulations, category sensitivities, consumer trust dynamics
- **The platform(s) selected** — algorithm behavior, ad policy, format constraints specific to that channel
- **The business size/capacity** — a handmade business creating demand it can't fulfill is a material risk; a SaaS company is not
- **The campaign goal** — re-engagement campaigns specifically risk confirming that subscribers are inactive; launch campaigns risk building expectation that can't be met

**Before (handmade candle brand, Instagram + Email, sales goal):**
"1. Creative fatigue: mitigate by varying content and capping frequency. 2. Budget overrun: set daily caps and review every 48 hours."

**After (same brief):**
"1. Demand-production mismatch: a launch campaign that works will generate orders faster than a handmade operation can fulfill. Risk: stockouts with no waitlist mechanism, customers left disappointed at the moment of highest intent. Mitigate: decide a launch inventory ceiling before the campaign starts, wire up a waitlist form, and frame limited availability as intentional scarcity rather than a logistics failure. 2. Instagram reach suppression on new content style: accounts that shift their posting format often see depressed organic reach for 2–4 weeks while the algorithm recalibrates. Mitigate: plan to evaluate organic reach only from week 3 onward; budget €80–120 to boost 2 top-performing posts in week 2 as insurance."

---

## Target Audience Psychographics

**The failure pattern:** Consumer psychology generalizations that apply to every premium buyer: "Likely to research before purchasing, values peer recommendations, responds well to authentic storytelling over hard selling."

**Why it happens:** These are the statistical-average traits of any considered-purchase consumer. The model generates the safest broadly-applicable description, which is also the least useful one.

**What good looks like:** Psychographics should identify the specific motivation or internal tension that makes *this* audience particularly receptive to *this* product. The useful question: what does this product represent to them beyond its functional use? What self-image does buying it support?

**Before (handmade botanical candles, 25–40 urban women):** "Motivated by warm & friendly brand experiences. Likely to research before purchasing, values peer recommendations, and responds well to authentic storytelling over hard selling."

**After:** "Treats home as a deliberate counter-environment to work — the candle is an act of curation, not decoration. Feels the tension between 'buy less, buy better' as an aspiration and the accumulated impulse purchases that contradict it. A handmade candle from a maker they can follow on Instagram resolves that tension: it's an indulgence that doesn't feel wasteful. The gifting occasion (birthday, housewarming) is the excuse; the maker story is the permission to pay the premium."
