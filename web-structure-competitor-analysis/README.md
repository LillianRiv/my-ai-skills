# web-structure-competitor-analysis

A Claude skill for analyzing and comparing the page structure and module layout of competitor websites. It produces a tiered comparison matrix, pattern-based insights, and actionable design strategy recommendations.

Fully industry-agnostic — works for e-commerce, SaaS, fintech, fashion, jewelry, education platforms, and beyond.

---

## What It Does

- **Collects competitors** — uses provided URLs or screenshots, or auto-identifies leading brands in the industry (minimum 10; scales to 15–20+ for brand-rich industries like jewelry or fashion)
- **Extracts page structure** — breaks each page into named modules from top to bottom
- **Clusters by tier** — groups competitors by market positioning and UX approach
- **Generates a comparison matrix** — rows = brands, columns = modules, color-coded by tier
- **Surfaces insights** — every finding follows a What / Why / So What structure
- **Recommends strategies** — 2–3 strategic directions tailored to the user's stated goal

---

## How to Use

1. Place `SKILL.md` in your Claude skills directory
2. Invoke the skill — Claude will ask **5 clarifying questions** before starting:
   - What industry?
   - Any competitor URLs or screenshots to include?
   - Is your own website included? *(required if redesigning)*
   - What is your goal? *(redesign / benchmarking / client presentation)*
   - Which page type? *(homepage / PDP / listing / other)*
3. Claude runs the full workflow and delivers a structured report

> Analyze one page type per session for best accuracy and depth.

---

## Output Structure

| Section | Content |
|---|---|
| Competitor List | Brand, URL, assigned tier, one-line rationale |
| Tier Definitions | Label, representative brands, core characteristics, user mindset |
| Module Matrix | Color-coded comparison table grouped by tier |
| Key Insights | 3–5 findings in What / Why / So What format |
| Strategy Recommendations | 2–3 strategy cards with module checklist, density guidance, trust-building approach |

---

## Matrix Color Convention

Tier rows are visually distinguished for readability:

| Tier | Markdown Prefix | HTML Background |
|---|---|---|
| Tier 1 — Premium / Brand-driven | 🔵 | `#EBF4FF` |
| Tier 2 — Hybrid | 🟡 | `#FFFBEB` |
| Tier 3 — Conversion-driven | 🔴 | `#FFF1F0` |

When competitors exceed 10, the matrix is split into separate tables per tier.

---

## Optional Enhancements

After the core report, Claude can offer:

- Wireframe prompts for each strategy direction
- Design tool prompts for Figma or image generation
- Slide-ready condensed summary for client presentations
- Direct gap analysis table (when user's own site is included)
- Continued analysis for the next page type (PDP, listing, etc.)

---

## Fallback Rules

- **Inaccessible website** → fall back to brand name + industry keyword search, or ask the user for a screenshot
- **Fewer than 10 competitors provided** → Claude supplements automatically to reach the minimum
- **User's own site included** → each strategy card includes a gap analysis field comparing current state to recommended direction

---

## File Structure

```
web-structure-competitor-analysis/
├── SKILL.md      # Full workflow instructions for Claude
└── README.md     # This file
```

---

## License

MIT — free to use, adapt, and share.
