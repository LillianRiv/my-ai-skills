---
name: web-design-strategy-builder
description: >
  Analyzes and compares the page structure and module layout of competitor websites.
  Identifies patterns in information architecture, module usage, and strategic positioning.
  Generates a structured comparison matrix, key insights, actionable design strategies,
  a section selector, a visual style vocabulary, and a final design tool prompt.
  Industry-agnostic. Works for any website type and any page type.
---

# Web Design Strategy Builder

## Overview

This skill analyzes and compares the **page structure and module layout** of competitor websites.
It identifies patterns in information architecture, module usage, and strategic positioning —
then generates a structured comparison matrix, key insights, and actionable design strategies.

The workflow is **industry-agnostic** and works for any website type (e-commerce, SaaS, content platforms, etc.).

Depending on the user's goal, the workflow extends beyond analysis into a full design preparation flow:
competitor analysis → positioning selection → brand information collection → section selector → visual style vocabulary → final prompt generation.

---

## Step 0 — Clarify Requirements (Always Required First)

Before doing anything else, ask the user these questions **in a single message** using `ask_user_input`:

1. **What industry is this?**
   (e.g., skincare, jewelry, fashion, SaaS, fintech, education)

2. **Do you have specific competitor websites to include?**
   - If yes → ask for URLs or screenshots
   - If no → AI will automatically identify top competitors (minimum 10)
   - If fewer than 10 are provided → AI will supplement to reach at least 10

3. **Do you have your own website to include in the analysis?**
   - If yes → ask for URL or screenshots
   - If no → skip (focus on competitor benchmarking only)
   - ⚠️ If the user's goal is to redesign their own site, their current site **must** be included

4. **What is your goal for this analysis?**
   Present as `ask_user_input` single_select with these options:
   - Redesign my own website
   - Identify industry best practices
   - Competitive monitoring / client presentation
   *(The goal determines whether brand information collection is triggered later — see Step 5A)*

5. **Which page type should be analyzed?**
   - Homepage
   - Product Detail Page (PDP)
   - Listing / Category Page
   - Other (specify)
   ⚠️ Strongly encourage analyzing **one page type at a time** for accuracy and depth.

---

## Step 1 — Competitor Selection

### If user provides websites:
- Use given URLs / screenshots as primary sources
- If fewer than 10 competitors are provided, supplement with additional leading brands

### If user provides no websites:
- Identify at least **10 leading brands** in the industry via web search
- Ensure a mix of market tiers:
  - High-end / premium
  - Mid-tier
  - Entry-level / mass market

### Fallback rule:
> ⚠️ If a website cannot be accessed (geo-block, login required, etc.),
> use brand name + industry keyword search to gather structural information,
> or ask the user to provide a screenshot.

### Output at this step:
A confirmed list of competitors with:
- Brand name
- **Clickable URL** (always include the live homepage link so the user can visit directly)
- Preliminary tier classification
- One-line rationale for tier placement

### After outputting the competitor list:
Ask the user to identify their **2–3 most relevant reference brands** using `ask_user_input` multi_select.
These selected brands will carry more weight in strategy recommendations and the final prompt.

> "Which of these brands feel most relevant or inspiring to your own project? Select 2–3."

---

## Step 2 — Structure Extraction

For **each competitor**, extract the page structure from top to bottom:

- Break the page into named modules
- Note the order and prominence of each module
- Flag any unusual or differentiating modules

### Common module types (non-exhaustive):

| Category | Module Examples |
|---|---|
| Entry / Awareness | Hero Section, Full-screen Video, Brand Statement |
| Navigation | Category Navigation, Mega Menu, Filters & Sorting |
| Product | Product Grid, Featured Collection, New Arrivals |
| Content | Editorial / Story Section, Lookbook, Blog |
| Trust | Reviews / Social Proof, Press Mentions, Certifications |
| Service | Shipping / Returns Info, Service Guarantees |
| CRM | Newsletter Signup, Loyalty Program, Membership |
| Conversion | Promotions Banner, Urgency Signals, CTA Blocks |
| Footer | Links, Contact, Social, Legal |

### Output at this step:
A structured module list per brand (top → bottom order).

---

## Step 3 — Tiering & Clustering

Group competitors based on observed patterns:

- Market positioning (price point, brand perception)
- UX structure approach (brand-driven vs. conversion-driven)
- Information density
- Trust-building method

### Default tier framework (adjust per industry):

| Tier | Label | Typical Characteristics |
|---|---|---|
| Tier 1 | Premium / Brand-driven | Sparse layout, editorial, storytelling-first |
| Tier 2 | Hybrid | Balance of brand and product, moderate density |
| Tier 3 | Conversion-driven | Dense product grid, promotions, functional CTAs |

> ⚠️ Tier definitions must reflect the specific industry. For SaaS, tiers might be
> Enterprise / SMB / Freemium. For fashion, Luxury / Contemporary / Fast Fashion.
> Always redefine tiers based on observed data, not assumed categories.

---

## Step 4 — Comparison Matrix

Create a matrix with:
- **Rows** = Competitors (grouped by tier, with a visual tier separator row)
- **Columns** = Modules

### Symbols:
| Symbol | Meaning |
|---|---|
| ✓ | Present and prominent |
| ~ | Present but weak / partial |
| — | Not present |

### Color coding for tier rows:

Use HTML table or Markdown with tier header rows styled as:
- **Tier 1** rows: prefix with `🔵`
- **Tier 2** rows: prefix with `🟡`
- **Tier 3** rows: prefix with `🔴`

Or, if generating in a rich format (HTML artifact / slide):
- Tier 1 rows → light blue background (`#EBF4FF`)
- Tier 2 rows → light yellow background (`#FFFBEB`)
- Tier 3 rows → light red/pink background (`#FFF1F0`)

### Example structure:

| Brand | Hero | Category Nav | Product Grid | Editorial | Reviews | CRM | Promotions |
|---|---|---|---|---|---|---|---|
| 🔵 **— Tier 1: Premium —** | | | | | | | |
| Brand A | ✓ | ~ | — | ✓ | — | ~ | — |
| Brand B | ✓ | — | — | ✓ | — | — | — |
| 🟡 **— Tier 2: Hybrid —** | | | | | | | |
| Brand C | ✓ | ✓ | ✓ | ✓ | ~ | ✓ | ~ |
| 🔴 **— Tier 3: Conversion —** | | | | | | | |
| Brand D | ~ | ✓ | ✓ | — | ✓ | ✓ | ✓ |

> ⚠️ Minimum 10 competitors. For industries with abundant brands (e.g., jewelry, fashion, beauty),
> aim for 15–20+. If competitors exceed 10, split the matrix by tier into separate tables for readability.

---

## Step 4B — Positioning Direction Selection (New)

After delivering the matrix, present **2–3 positioning directions** derived from the analysis.
Do NOT ask the user to name their tier upfront — let the analysis inform the options first.

Each direction should:
- Reference specific competitor brands the user recognizes (e.g., "closer to Aesop / Sisley" rather than "Tier 1")
- Describe the structural and tonal implications in plain language
- Be presented as a distinct strategic path, not just a tier label

Use `ask_user_input` single_select to let the user choose their direction.

### Example format:

> Based on the competitive landscape, here are three positioning directions for your brand:
>
> **A — Premium / Brand-led** (closer to Aesop, Sisley)
> Sparse layout, editorial storytelling, no promotions. Trust built through heritage and ingredient philosophy.
>
> **B — Hybrid Elevation** (closer to Tatcha, Fresh)
> Balance of brand narrative and product discovery. Moderate density. Appeals to both new and loyal buyers.
>
> **C — Science / Performance-led** (closer to SkinCeuticals, Augustinus Bader)
> Credibility through clinical proof points, awards, and proprietary technology. Data-driven trust signals.
>
> Which direction feels closest to where you want to position your brand?

---

## Step 5 — Pattern Analysis & Strategic Insights

*(Steps 5 and 6 are merged: diagnosis first, then direction)*

### Part A — Pattern Observations

For each pattern, use this fixed structure:

> **What:** What is observed across competitors
> **Why:** Why this structural choice likely exists
> **So what:** What this means for our design decisions

Cover these dimensions:

#### 1. Module Distribution by Tier
- Which modules are universal (appear across all tiers)?
- Which modules are tier-exclusive?
- Which modules are unexpected / differentiating?

#### 2. Information Density
- How dense is the above-the-fold content per tier?
- Product count, text volume, visual complexity

#### 3. Trust-Building Mechanisms
- Tier 1: storytelling, heritage, craftsmanship language
- Tier 2: a mix of editorial and functional reassurance
- Tier 3: reviews, ratings, return policies, urgency signals

#### 4. Conversion Patterns
- Where and how CTAs appear
- Use of promotions / urgency
- How product discovery is structured

### Part B — Strategic Directions

Based on the user's **chosen positioning direction** (from Step 4B), provide **2–3 strategic directions**.

Each strategy uses this fixed card format:

---
**Strategy Name:** *(one-line label, e.g., "Brand Elevation")*

- **Best for:** When to choose this strategy
- **Module checklist:** Modules to add / keep / remove
- **Information density:** Dense / moderate / sparse; above-the-fold priority
- **Trust-building approach:** How to build trust at this tier
- **Gap vs. current site:** *(Only if user's own site is included)* Key gaps to address
---

---

## Step 5A — Brand Information Collection (Conditional)

Trigger this step **only if** the user's goal (from Step 0) is:
- "Redesign my own website", OR
- "Competitive monitoring / client presentation"

**Skip entirely** if the goal is "Identify industry best practices".

Collect the following in **three focused questions**. All fields are optional — if the user skips a field, use placeholder descriptions in the final prompt instead.

**Question 1 — Brand identity**
> What is your brand name? Do you have a tagline or core brand statement?
> *(Used for: logo reference in nav, hero headline direction, footer)*

**Question 2 — Existing brand assets**
> Which of the following do you already have? (multi_select)
> - Brand colors / hex values
> - Defined typeface / font
> - Product photography or brand imagery
> - Existing website
> - None of the above
> *(Used for: visual style section — confirmed assets override vocabulary suggestions)*

**Question 3 — Target audience**
> Describe your core target audience in one sentence.
> (e.g., "Urban women aged 25–35 who prioritize ingredient transparency" / "Product managers at mid-size SaaS companies")
> *(Used for: photography direction, copy tone, imagery subject description in the prompt)*

---

## Step 6 — Section Selector

After the strategy is confirmed and brand information collected (if applicable), present the **Section Selector**.

This step lets the user decide which homepage sections to include in the final prompt.

### 6A — Select mode

Use `ask_user_input` single_select:

> How would you like to proceed?
> - **Follow the recommended strategy** — use the pre-selected sections based on your chosen direction
> - **Customize my own selection** — choose sections manually

### 6B — Generate section list

Based on the confirmed industry and positioning direction, generate a section list specific to that context. Do not use a generic list — the sections, their descriptions, and their recommended tiers must reflect the industry.

Each section entry includes:
- Section name (English + local language if applicable)
- One-line description of its role and design guidance
- Recommendation tier: **Core** / **Optional** / **Advanced** / **Not recommended**
- Whether it is included in the chosen strategy by default

### 6C — Present sections for selection

**If user chose "Follow the recommended strategy":**
Output the pre-selected section list as plain text. Sections included in the strategy are clearly marked. No interaction needed — proceed directly to Step 7.

**If user chose "Customize my own selection":**
Present all sections (excluding "Not recommended" unless the user requests them) using `ask_user_input` multi_select. Group by tier in the label for clarity.

> Example option labels:
> - `[Core] Full-screen Hero — cinematic image or video, single CTA, no product prices`
> - `[Core] Brand Philosophy — two-column origin story, earns the price point`
> - `[Optional] Editorial / Library — 2–3 article teasers, timeless feel`
> - `[Advanced] Ritual / How-to — step-by-step usage guide, suitable for multi-SKU brands`

The order of sections in the list = the recommended top-to-bottom page order.

### Supported industries with pre-built section sets:
- Skincare
- Fashion
- Jewelry
- Home & Living
- SaaS

For any other industry, generate the section list dynamically based on industry conventions and the chosen positioning tier.

---

## Step 7 — Visual Style Vocabulary

After the section list is confirmed, present the **Visual Style Vocabulary Selector** using the `visualize` tool (rendered inline widget).

The vocabulary covers **seven dimensions**. Each dimension's options are industry-adapted — do not use generic or cross-industry terms.

| Dimension | Description |
|---|---|
| Overall style archetype | The brand world and aesthetic reference (e.g., Apothecary ritual, Japandi, Clinical science) |
| Color palette direction | 3-stop color ramps with visual swatches |
| Typography style | Font personality and reference typefaces |
| Photography / imagery style | Shooting style, subject, lighting, mood |
| Layout rhythm | Density, whitespace approach, page pacing |
| Decorative language | Details, ornaments, typographic moments |
| Motion & interaction | Animation presence and character |

### Supported industries with pre-built vocabulary sets:
- Skincare
- Fashion
- Jewelry
- Home & Living
- SaaS

For any other industry, use `sendPrompt()` to trigger AI generation of a matching vocabulary set in the same format, then present it for selection.

### Rendering note:
> ⚠️ The visual style vocabulary **must** use the `visualize` tool for rendering. Do not fall back to plain text lists for this step. If the visualize tool times out, inform the user and offer to re-render or proceed with a text-based selection instead.

---

## Step 8 — Final Prompt Generation

Assemble all collected inputs into a single, complete design prompt ready to paste into the user's target tool.

### 8A — Ask for target tool

Use `ask_user_input` single_select:

> Which design tool will you use to generate the homepage?
> - Google Stitch
> - Figma AI
> - Framer AI
> - Other (I'll adapt the prompt myself)

### 8B — Ask for primary device target

Use `ask_user_input` single_select:

> Should the design prioritize mobile or desktop layout?
> - Desktop first
> - Mobile first

### 8C — Assemble the final prompt

The prompt must integrate all of the following inputs in order:

1. **Industry + positioning tier** — sets the overall register and reference frame
2. **Reference competitors** — the 2–3 brands the user selected in Step 1 as most relevant
3. **Brand information** — name, tagline, target audience, confirmed assets *(if collected in Step 5A)*
4. **Section list** — in page order, each with a one-paragraph design description
5. **Visual style vocabulary** — all selected dimensions woven into the visual direction description
6. **Target tool** — adjust prompt structure and terminology to match the tool's input format
7. **Device priority** — specify layout starting point
8. **Explicit exclusions** — any elements flagged as "not recommended" for the chosen tier (e.g., promotions banners, star rating blocks, urgency signals)

### Prompt format guidelines by tool:

| Tool | Format guidance |
|---|---|
| Google Stitch | Structured prose sections per module. Clear visual direction per section. Can reference named brand aesthetics. |
| Figma AI | Component-oriented language. Reference specific layout patterns (two-column, full-bleed, grid). |
| Framer AI | Can include interaction notes. Describe scroll behavior and hover states where relevant. |
| Other | Use clean structured prose. No tool-specific terminology. |

---

## Step 9 — Output Format

Deliver the full report in this order:

### 1. Competitor List
- Brand, clickable URL, assigned tier, one-line rationale

### 2. Tier Definitions
For each tier:
- **Tier name**
- **Representative brands**
- **Core characteristics** (max 3 bullet points)
- **Typical user mindset / expectation**

### 3. Module Comparison Matrix
- Color-coded by tier (see Step 4 for formatting rules)
- Tier separator rows bolded

### 4. Key Insights
- 3–5 insights using the **What / Why / So What** structure
- Lead with the most strategically important finding

### 5. Strategy Recommendations
- 2–3 strategy cards (see Step 5 format)
- Tag each card with the user goal it best serves

### 6. *(If applicable)* Section List + Visual Style Summary
- Confirmed section order
- Selected visual style dimensions summarized in 2–3 sentences

### 7. *(If applicable)* Final Design Prompt
- Ready to copy and paste into the target tool

---

## Optional Enhancements

After delivering the core report, offer these if relevant:

- **Wireframe prompts** — AI-generated layout descriptions for each strategy direction
- **Slide-ready summary** — Condensed version suitable for client presentation
- **Next page analysis** — Offer to repeat the workflow for a different page type (PDP, PLP, etc.)
- **Gap analysis table** — If user's own site is included, a direct before/after comparison

---

## Notes

- Focus on **structure and module logic**, not visual style or aesthetics (until Step 7)
- Prioritize **patterns across tiers** over individual brand quirks
- Ensure all insights are **actionable**, not merely descriptive
- Keep the analysis **scalable** — the same workflow applies whether this is jewelry, SaaS, or fintech
- If a website cannot be accessed, use web search with brand name + industry keywords as fallback
- Always analyze **one page type at a time** — do not mix homepage and PDP analysis
- Brand positioning direction is always derived **from the analysis**, never asked upfront
- Brand information collection is **conditional** — only trigger for "redesign" or "client presentation" goals
- The `visualize` tool is used **only** for the visual style vocabulary (Step 7) — all selectors and inputs use `ask_user_input`
