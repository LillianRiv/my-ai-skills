# html-to-shopify-dawn

An AI skill for converting any HTML design into Shopify Dawn theme Liquid code — including custom sections, style unification, header/footer overrides, and Theme Editor schema.

---

## What this skill does

- Extracts brand tokens (colors, fonts, spacing) from your HTML
- Generates a Global CSS Unification block so Dawn's native sections match your custom ones
- Classifies every section: use Dawn native, override, or build custom
- Converts HTML → Shopify Liquid with full `{% schema %}`, settings, and blocks
- Handles header layout, footer grid, icon bars (SVG only), dark editorial sections, card grids
- Enforces Shopify's absolute rules (never break cart, checkout, or `content_for_layout`)

Designed for any HTML — static mockups, Figma exports, Bootstrap layouts, or hand-coded pages.

---

## How to use

### Claude (claude.ai) — manual upload
Upload `SKILL.md` inside a Project. It will be available to every conversation in that project.

### Claude Code — automatic
Place `SKILL.md` in your Shopify theme repo and reference it from your `CLAUDE.md`:
```
@SKILL.md
```
Claude Code reads `CLAUDE.md` automatically at the start of every session.

### Cursor — automatic
Copy `SKILL.md` to `.cursor/rules/html-to-shopify-dawn.mdc` inside your Shopify theme repo. Cursor loads rules from `.cursor/rules/` automatically.

### Codex (CLI) — automatic
Rename to `AGENTS.md` and place it in your Shopify theme repo root. Codex reads `AGENTS.md` automatically before starting any task.

---

## Workflow overview

```
Phase 0 — Brand token extraction (colors, fonts, spacing)
Phase 1 — Global CSS Unification block (native + custom sections match)
Phase 2 — Design analysis (inventory sections, classify each one)
Phase 3 — Section conversion (native override or custom Liquid)
Phase 4 — CSS strategy (specificity, !important rules, gotchas)
Phase 5 — Header layout (logo_position, double-row nav)
Phase 6 — Footer layout (grid override, safe zones)
Phase 7 — Output format (file path → code → editor steps → changelog)
```

---

## Requirements

- Shopify Dawn theme (any recent version)
- An HTML file or design to convert

---

## License

MIT
