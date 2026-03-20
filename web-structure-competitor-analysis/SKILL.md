---
name: web-structure-competitor-analysis
description: >
  Analyzes and compares the page structure and module layout of competitor websites.
  Identifies patterns in information architecture, module usage, and strategic positioning.
  Generates a structured comparison matrix, key insights, and actionable design strategies.
  Industry-agnostic. Works for any website type and any page type.
---

# Web Structure Competitor Analysis

## Overview

This skill analyzes and compares the **page structure and module layout** of competitor websites.
It identifies patterns in information architecture, module usage, and strategic positioning —
then generates a structured comparison matrix, key insights, and actionable design strategies.

The workflow is **industry-agnostic** and works for any website type (e-commerce, SaaS, content platforms, etc.).

---

## Step 0 — Clarify Requirements (Always Required First)

Before doing anything else, ask the user these questions **in a single message**:

1. **What industry is this?**
   (e.g., jewelry, fashion, SaaS, fintech, education)

2. **Do you have specific competitor websites to include?**
   - If yes → ask for URLs or screenshots
   - If no → AI will automatically identify top competitors (minimum 10)
   - If fewer than 10 are provided → AI will supplement to reach at least 10

3. **Do you have your own website to include in the analysis?**
   - If yes → ask for URL or screenshots
   - If no → skip (focus on competitor benchmarking only)
   - ⚠️ If the user's goal is to redesign their own site, their current site **must** be included

4. **What is your goal for this analysis?**
   Examples:
   - Redesign my own website
   - Identify industry best practices
   - Competitive monitoring / client presentation
   *(The goal shapes the strategy recommendations in Step 6)*

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
A confirmed list of competitors with brand name, URL, and preliminary tier classification.

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

## Step 5 — Pattern Analysis & Strategic Insights

*(Steps 5 and 6 are merged: diagnosis first, then direction)*

### Part A — Pattern Observations

For each pattern, use this fixed structure:

> **现象 (What):** What is observed across competitors  
> **原因推断 (Why):** Why this structural choice likely exists  
> **设计含义 (So what):** What this means for our design decisions

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

Based on the user's **stated goal** (from Step 0, Question 4), provide **2–3 strategic directions**.

Each strategy uses this fixed card format:

---
**Strategy Name:** *(one-line label, e.g., "Brand Elevation")*

- **适合场景:** When to choose this strategy
- **模块增删清单:** Modules to add / keep / remove
- **信息密度建议:** Dense / moderate / sparse; above-the-fold priority
- **信任建立方式:** How to build trust at this tier
- **与用户当前网站的差距:** *(Only if user's own site is included)* Key gaps to address
---

> ⚠️ Before outputting strategies, confirm with the user:
> "你的品牌目前属于哪个tier？或者你希望往哪个方向定位？"
> This ensures strategy recommendations are anchored to their actual context.

---

## Step 6 — Output Format

Deliver the full report in this order:

### 1. Competitor List
- Brand, URL, assigned tier, one-line rationale for tier placement

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

---

## Optional Enhancements

After delivering the core report, offer these if relevant:

- **Wireframe prompts** — AI-generated layout descriptions for each strategy direction
- **Design tool prompts** — Prompts for Figma / Midjourney to visualize the recommended structure
- **Slide-ready summary** — Condensed version suitable for client presentation
- **Next page analysis** — Offer to repeat the workflow for a different page type (PDP, PLP, etc.)
- **Gap analysis table** — If user's own site is included, a direct before/after comparison

---

## Notes

- Focus on **structure and module logic**, not visual style or aesthetics
- Prioritize **patterns across tiers** over individual brand quirks
- Ensure all insights are **actionable**, not merely descriptive
- Keep the analysis **scalable** — the same workflow applies whether this is jewelry, SaaS, or fintech
- If a website cannot be accessed, use web search with brand name + industry keywords as fallback
- Always analyze **one page type at a time** — do not mix homepage and PDP analysis
