# web-design-strategy-builder

A Claude skill that takes you from competitive analysis to a ready-to-use design prompt — in one guided conversation.

It analyzes competitor page structure, identifies positioning patterns, helps you define your brand direction, and generates a fully assembled prompt for your design tool of choice (Google Stitch, Figma AI, Framer AI, etc.).

Fully industry-agnostic — works for skincare, fashion, jewelry, home & living, SaaS, and any other industry.

---

## What It Does

- **Analyzes competitors** — uses provided URLs or auto-identifies leading brands (minimum 10), with clickable links so you can browse them directly
- **Extracts page structure** — breaks each page into named modules from top to bottom
- **Clusters by tier** — groups competitors by market positioning and UX approach
- **Generates a comparison matrix** — rows = brands, columns = modules, color-coded by tier
- **Surfaces insights** — every finding follows a What / Why / So What structure
- **Presents positioning directions** — 2–3 options derived from the analysis, referenced to brands you recognize, so you can choose where to position your brand
- **Collects brand information** — name, tagline, existing assets, and target audience *(only when relevant to your goal)*
- **Section selector** — lets you confirm or customize which page sections to include, with AI-generated recommendations per industry and positioning tier
- **Visual style vocabulary** — an interactive selector across 7 dimensions (style archetype, color palette, typography, photography, layout rhythm, decorative language, motion), fully adapted to your industry
- **Generates the final prompt** — integrates all inputs into a single, copy-paste-ready prompt formatted for your target design tool

---

## How to Use

1. Place `SKILL.md` in your Claude skills directory
2. Invoke the skill — Claude will ask **5 clarifying questions** before starting:
   - What industry?
   - Any competitor URLs or screenshots to include?
   - Is your own website included? *(required if redesigning)*
   - What is your goal? *(redesign / benchmarking / client presentation)*
   - Which page type? *(homepage / PDP / listing / other)*
3. Claude runs the competitive analysis and delivers the report
4. You choose your positioning direction based on the analysis
5. *(If redesigning or preparing a client presentation)* Answer 3 quick brand questions
6. Confirm or customize your page sections
7. Select your visual style across 7 dimensions
8. Receive your final design prompt, formatted for your tool

> Analyze one page type per session for best accuracy and depth.

---

## Workflow Overview

```
Step 0 — Clarify requirements (industry, goal, page type)
    ↓
Step 1 — Competitor selection + clickable URLs + user marks 2–3 reference brands
    ↓
Step 2 — Structure extraction (module list per brand, top → bottom)
    ↓
Step 3 — Tiering & clustering (Premium / Hybrid / Conversion-driven)
    ↓
Step 4 — Comparison matrix (color-coded by tier)
    ↓
Step 4B — Positioning direction selection (brand-referenced options, not abstract tier labels)
    ↓
Step 5 — Pattern analysis & strategy recommendations
    ↓
Step 5A — Brand information collection [conditional: redesign or client presentation goals only]
    ↓
Step 6 — Section selector (follow strategy or customize via multi-select)
    ↓
Step 7 — Visual style vocabulary (interactive 7-dimension selector)
    ↓
Step 8 — Final prompt generation (tool-adapted, device-aware, exclusions included)
```

---

## Output Structure

| Section | Content |
|---|---|
| Competitor list | Brand, clickable URL, assigned tier, one-line rationale |
| Tier definitions | Label, representative brands, core characteristics, user mindset |
| Module matrix | Color-coded comparison table grouped by tier |
| Key insights | 3–5 findings in What / Why / So What format |
| Strategy recommendations | 2–3 strategy cards with module checklist, density guidance, trust-building approach |
| *(If applicable)* Section list | Confirmed sections in page order with design guidance per section |
| *(If applicable)* Visual style summary | Selected dimensions summarized in 2–3 sentences |
| *(If applicable)* Final design prompt | Ready to paste into Google Stitch, Figma AI, Framer AI, or other tools |

---

## Conditional Logic

| User goal | Brand info collected? | Section selector shown? | Visual vocabulary shown? | Final prompt generated? |
|---|---|---|---|---|
| Identify industry best practices | No | No | No | No |
| Redesign my own website | Yes | Yes | Yes | Yes |
| Competitive monitoring / client presentation | Yes | Yes | Yes | Yes |

---

## Section Selector

Sections are generated dynamically based on the confirmed industry and positioning direction. Each section includes:

- Name and one-line design description
- Recommendation tier: **Core** / **Optional** / **Advanced** / **Not recommended**
- Whether it is included in the chosen strategy by default

Two modes available:
- **Follow recommended strategy** — pre-selected sections, proceed directly
- **Customize** — free multi-select via `ask_user_input`

Pre-built section sets are available for: Skincare, Fashion, Jewelry, Home & Living, SaaS.
For all other industries, Claude generates the section list dynamically.

---

## Visual Style Vocabulary

An interactive selector rendered inline, covering 7 dimensions:

| Dimension | Examples |
|---|---|
| Overall style archetype | Apothecary ritual, Japandi, Clinical science, Couture luxury… |
| Color palette direction | Warm neutral, Ivory & black, Deep forest, Midnight navy… *(with color swatches)* |
| Typography style | High-contrast serif, Geometric sans, Monospace / lab… |
| Photography / imagery style | Close-up texture, Still life on linen, Moody shadow… |
| Layout rhythm | Ultra sparse, Editorial asymmetry, Full-bleed sections… |
| Decorative language | Hairline rules, Large type moments, Botanical illustration… |
| Motion & interaction | No motion, Slow parallax, Cinematic video hero… |

Vocabulary sets are industry-adapted. For unlisted industries, Claude generates a matching set on demand.

---

## Final Prompt Integration

The generated prompt includes all of the following:

1. Industry + positioning tier
2. Reference competitors (user-selected 2–3 most relevant brands)
3. Brand information — name, tagline, target audience, confirmed assets *(if collected)*
4. Section list — in page order, each with a design description
5. Visual style selections — woven into a cohesive visual direction paragraph
6. Target tool formatting — adapted to Google Stitch, Figma AI, or Framer AI
7. Device priority — desktop-first or mobile-first
8. Explicit exclusions — elements flagged as conflicting with the chosen positioning tier

---

## Matrix Color Convention

| Tier | Markdown prefix | HTML background |
|---|---|---|
| Tier 1 — Premium / Brand-driven | 🔵 | `#EBF4FF` |
| Tier 2 — Hybrid | 🟡 | `#FFFBEB` |
| Tier 3 — Conversion-driven | 🔴 | `#FFF1F0` |

When competitors exceed 10, the matrix is split into separate tables per tier for readability.

---

## Fallback Rules

- **Inaccessible website** → fall back to brand name + industry keyword search, or ask for a screenshot
- **Fewer than 10 competitors provided** → Claude supplements automatically to reach the minimum
- **User's own site included** → each strategy card includes a gap analysis field
- **Unlisted industry** → section list and visual vocabulary are generated dynamically
- **Visualize tool timeout** → Claude informs the user and offers to re-render or switch to text-based selection for the visual vocabulary step

---

## File Structure

```
web-design-strategy-builder/
├── SKILL.md      # Full workflow instructions for Claude
└── README.md     # This file
```

---

## License

MIT — free to use, adapt, and share.
