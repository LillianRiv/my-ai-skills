# html-to-shopify-dawn

An AI skill for converting any HTML design into Shopify Dawn theme Liquid code — including custom sections, style unification, header/footer overrides, and Theme Editor schema.

Works with **Claude**, **Cursor**, and **OpenAI Codex** (see version files below).

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

## Files

| File | Use with |
|---|---|
| `claude/SKILL.md` | Claude (claude.ai, Claude Code, API) |
| `cursor/SKILL.md` | Cursor IDE |
| `codex/SKILL.md` | OpenAI Codex / ChatGPT with code context |

---

## How to use

### Claude (claude.ai)

Upload `claude/SKILL.md` as a Project file or paste its contents into your system prompt. Then share your HTML and say:

> "Convert this HTML design to a Shopify Dawn theme."

### Cursor

Copy `cursor/SKILL.md` to `.cursor/rules/html-to-shopify-dawn.md` in your Shopify theme repo. The rule will activate automatically when you work on `.liquid` and `.css` files.

### Codex / ChatGPT

Paste the contents of `codex/SKILL.md` as a system prompt or prepend it to your first message. Works with GPT-4o and o-series models.

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

Reference sections include Dawn's native class names, safe override points, and reusable Liquid snippet templates.

---

## Requirements

- Shopify Dawn theme (any recent version)
- An HTML file or design to convert

---

## License

MIT
