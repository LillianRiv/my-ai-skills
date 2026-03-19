---
name: html-to-shopify-dawn
description: >
  Converts any HTML design into Shopify Dawn theme Liquid code. Use this skill
  whenever the user wants to implement an HTML/CSS design into a Shopify Dawn theme,
  add or modify sections, fix CSS inconsistencies across native and custom sections,
  or troubleshoot layout issues in Dawn. Triggers on phrases like "convert this HTML",
  "add this section to Shopify", "implement this in Dawn", "fix the header/footer layout",
  "unify styles across sections", or any time an HTML file is provided alongside a
  Shopify context. Always use this skill before writing any Liquid or CSS for Dawn.
---

# Shopify Dawn — HTML to Liquid Converter

## Phase 0 — Brand Token Extraction (run FIRST, every time)

Before writing a single line of code, extract the brand system from the provided HTML.
Ask the user to confirm if anything is ambiguous.

Extract and record:
```
Primary color:       (e.g. #800020)
Primary hover:       (darken primary by ~10%)
Background color:    (body bg)
Dark background:     (if used)
Heading font:        (name + weight + style)
Body font:           (name + weight)
Button style:        (radius, padding, text-transform, letter-spacing)
Decorative elements: (dividers, ornaments, special patterns)
Spacing unit:        (base padding/gap used throughout)
```

Then immediately produce the **Global CSS Block** — a single `base.css` append that defines:
1. CSS custom properties (`--color-primary`, `--color-bg`, etc.)
2. All reusable utility classes (`.btn-primary`, `.btn-outline`, `.text-primary`, etc.)
3. The Global Unification Block (see Phase 1)

**This block is written once and never touched again.** All subsequent sections reference it.

---

## Phase 1 — Global Style Unification (always included)

Every project needs this block in `assets/base.css`. It ensures Dawn's native sections
(Image Banner, Featured Collection, Email Signup, etc.) match the custom sections visually.

Template — fill in values from Phase 0:

```css
/* ===== [BRAND NAME] GLOBAL STYLE UNIFICATION ===== */

/* Body font follows Theme Editor variable */
body, .rte, .rte p, p, li, span, a,
input, textarea, select, button, .field__input {
  font-family: var(--font-body-family) !important;
}

/* Headings follow Theme Editor variable */
h1,h2,h3,h4,h5,h6,
.h1,.h2,.h3,.h4,.h5,.h6,
.featured-collection__title, .multicolumn__title,
.newsletter__heading, .title {
  font-family: var(--font-heading-family) !important;
  font-weight: var(--font-heading-weight) !important;
  font-style: var(--font-heading-style) !important;
}

/* Primary button — override ALL Dawn native section buttons */
.button, .button--primary,
button[type="submit"], input[type="submit"],
.shopify-payment-button__button {
  background-color: [PRIMARY COLOR] !important;
  color: #ffffff !important;
  border: none !important;
  border-radius: [RADIUS] !important;
  font-family: var(--font-body-family) !important;
  font-size: [SIZE] !important;
  font-weight: [WEIGHT] !important;
  text-transform: [TRANSFORM] !important;
  letter-spacing: [SPACING] !important;
  padding: [PADDING] !important;
  transition: background 0.2s !important;
}
.button:hover, .button--primary:hover,
button[type="submit"]:hover {
  background-color: [PRIMARY HOVER] !important;
}

/* Secondary button */
.button--secondary {
  background-color: transparent !important;
  border: 1px solid [BORDER COLOR] !important;
  border-radius: [RADIUS] !important;
  /* ... */
}

/* Newsletter button */
.newsletter-form__button {
  background-color: [PRIMARY COLOR] !important;
  border-radius: 0 [RADIUS] [RADIUS] 0 !important;
}

/* Input fields */
.field__input, input[type="email"], input[type="text"] {
  border-color: [BORDER COLOR] !important;
  border-radius: [RADIUS] 0 0 [RADIUS] !important;
  font-family: var(--font-body-family) !important;
}
.field__input:focus, input[type="email"]:focus {
  border-color: [PRIMARY COLOR] !important;
  box-shadow: 0 0 0 1px [PRIMARY COLOR] !important;
}

/* Focus ring */
:focus-visible {
  outline: 2px solid [PRIMARY COLOR] !important;
  outline-offset: 2px !important;
}
```

**Why use `var(--font-body-family)` instead of hardcoding?**
Shopify injects font variables from the Theme Editor. Using variables means the merchant
can change fonts in the editor and all sections update automatically — both native and custom.

---

## Phase 2 — Design Analysis

Before converting any section, analyze the full HTML design:

1. **Inventory all sections** — list every distinct visual block top to bottom
2. **Classify each section**:
   - ✅ Dawn native covers it → use native + CSS override
   - 🔨 Dawn native is close → modify existing section minimally
   - 🆕 No Dawn equivalent → create new custom section
3. **Map the homepage order** → plan `templates/index.json`
4. **Identify shared patterns** → extract into utility classes, not repeated inline styles

| Section type | Dawn native | Action |
|---|---|---|
| Full-width hero / image banner | `image-banner` | Use native, add CSS |
| Product grid | `featured-collection` | Use native, add CSS |
| Newsletter signup | `email-signup` | Use native, add CSS |
| Trust/icon bar | `multicolumn` | Check if horizontal — if not, create custom |
| Editorial (dark bg, left/right) | None | Create custom |
| Category image grid | None | Create custom |
| Store locator / boutique | None | Create custom |
| Events / editorial cards | None | Create custom |

---

## Phase 3 — Section Conversion Rules

### 3a. For Dawn Native Sections
- **Never rewrite the Liquid logic**
- Only wrap output in CSS classes or add minimal HTML wrappers
- Override styles in `assets/base.css` or the section's own CSS file
- To add a decorative element (e.g. a divider), find the nearest safe insertion point
  in the HTML output and insert static HTML — never inside `{% form %}` blocks

### 3b. For Custom Sections
Follow this order every time:

**Step 1 — Structure**
```liquid
<section>
  <!-- Layout HTML here -->
  <!-- Use CSS Grid or Flexbox via inline style for layout -->
  <!-- Use utility classes (from Phase 0) for colors, buttons, fonts -->
</section>
```

**Step 2 — Replace all static content with Liquid**

| Static HTML | Liquid replacement |
|---|---|
| `<img src="...">` | `{{ section.settings.image \| image_url: width: N \| image_tag: style: '...' }}` |
| Hardcoded text | `{{ section.settings.heading }}` |
| `href="#"` | `{{ section.settings.button_url }}` |
| Inline `color:` style | Replace with utility class from Phase 0 |
| Inline `font-family:` style | Remove — handled by Global Unification block |
| Inline `font-style: italic` | Remove — handled by heading CSS variable |
| Background image inline | `style="background-image: url('{{ section.settings.image \| image_url: width: 1600 }}')"` |

**Step 3 — Add `{% schema %}`**
- Every text, image, URL → becomes a schema setting
- Every repeating item (cards, events, categories) → becomes a `blocks` entry
- Always add `"presets"` so section appears in the Add Section menu
- Add `{{ block.shopify_attributes }}` to each block's root element

**Step 4 — Image containers**
Always wrap images in a fixed-height container. Never rely on `aspect-ratio` on `<img>`:
```html
<!-- ✅ Correct — consistent height regardless of source image dimensions -->
<div style="height: 220px; overflow: hidden; border-radius: Xpx;">
  {{ block.settings.image | image_url: width: 600 | image_tag:
      style: 'width:100%; height:100%; object-fit:cover; display:block;' }}
</div>

<!-- ❌ Avoid — breaks when source images have different aspect ratios -->
{{ image | image_tag: style: 'aspect-ratio: 16/9; object-fit:cover;' }}
```

**Step 5 — Icons**
Always use inline SVG. Never use icon font classes (Material Icons, Font Awesome etc.)
in Shopify sections — Dawn's global CSS overrides `font-family` and breaks them.
If the design uses icon fonts, convert to SVG equivalents.
If the icon set is large, use a `select` schema field to let the merchant pick icons
from a predefined set, and render with `{% case %}`.

---

## Phase 4 — CSS Strategy

### Specificity hierarchy (from lowest to highest):
1. Dawn base styles
2. Section-specific CSS files (`section-footer.css` etc.)
3. `assets/base.css` appended block (our overrides)
4. Inline `style=""` attributes (use sparingly, only for dynamic values)

### Rules:
- **Always append** — never modify existing CSS rules above your block
- **Add `!important`** when overriding Dawn native classes (Dawn uses high specificity)
- **Prefix selectors** with a parent class to avoid unintended side effects:
  e.g. `.footer .list-social` not just `.list-social`
- **Discover the real class name** before writing overrides — Dawn's actual class names
  often differ from what you'd guess. When in doubt, ask the user to inspect element
  or paste the relevant CSS file

### Common Dawn CSS gotchas:
| Issue | Real cause | Fix |
|---|---|---|
| Social icons vertical | Class is `.list-social` not `.footer__list-social` | Target `.footer .list-social { flex-direction: row !important }` |
| Footer links wrapping | `grid__item` has implicit max-width | Add `width: 100% !important; max-width: 100% !important` to `.grid__item` |
| Heading font not applying | Dawn wraps in `<span>` with own class | Target both `h2` AND `.h2` |
| Icon font shows as text | Dawn CSS overrides `font-family` | Convert to SVG |
| Button wrong color in native section | `.button` class has high specificity | Use `!important` in Global Unification block |
| Image heights unequal across cards | `aspect-ratio` on `<img>` is unreliable | Use fixed-height wrapper div |
| Sticky header not blurring | Class only applied on scroll | Target `.shopify-section-header-sticky .header-wrapper` |

---

## Phase 5 — Header Layout

Dawn header layout is controlled by `logo_position` setting.
The CSS class added to `<header>` is `header--[logo_position_value]`.

| `logo_position` value | Class | Nav position |
|---|---|---|
| `middle-left` | `header--middle-left` | Right of logo, same row |
| `middle-center` | `header--middle-center` | Left of logo, same row by default |
| `top-left` | `header--top-left` | Below logo |
| `top-center` | `header--top-center` | Below logo |

**To force nav below logo (double-row) for `middle-center`**, use CSS Grid:
```css
@media screen and (min-width: 990px) {
  .header--middle-center {
    display: grid !important;
    grid-template-areas:
      "empty logo icons"
      "nav   nav  nav  " !important;
    grid-template-columns: 1fr auto 1fr !important;
    grid-template-rows: auto auto !important;
    padding-bottom: 0 !important;
  }
  .header--middle-center .header__heading  { grid-area: logo !important; justify-self: center; }
  .header--middle-center .header__icons    { grid-area: icons !important; justify-self: end; }
  .header--middle-center header-drawer     { grid-area: empty !important; }
  .header--middle-center .header__inline-menu,
  .header--middle-center .header__menu-wrapper {
    grid-area: nav !important;
    display: flex !important;
    justify-content: center !important;
    border-top: 1px solid [BORDER COLOR] !important;
    width: 100% !important;
  }
}
```

**Never touch in `sections/header.liquid`**:
- Cart icon and its `cart-count-bubble`
- Account link
- Search modal
- `sticky-header` JavaScript class

---

## Phase 6 — Footer Layout

Dawn footer is fully controlled by `sections/footer.liquid` + blocks in the Theme Editor.

**Never touch**:
- `{%- form 'customer', id: 'ContactFooter' -%}` — newsletter form
- `shop.enabled_payment_types` loop — payment icons
- `shop.policies` loop — legal links
- `{{ powered_by_link }}` — required by Shopify
- `{% render 'country-localization' %}` — multi-currency

**Safe to modify**:
- CSS classes on structural wrappers
- Appending CSS to `assets/section-footer.css`
- Adding static decorative HTML (brand wordmark, dividers) as siblings to existing divs

**Footer grid**: Dawn auto-assigns grid classes based on block count.
Override ALL variants to ensure your column count always wins:
```css
.footer__content-top .grid.footer__blocks-wrapper,
.footer__content-top .grid--2-col.footer__blocks-wrapper,
.footer__content-top .grid--4-col-tablet.footer__blocks-wrapper,
.footer__content-top .grid--3-col-tablet.footer__blocks-wrapper,
.footer__content-top .grid--4-col-desktop.footer__blocks-wrapper {
  display: grid !important;
  grid-template-columns: repeat([N], 1fr) !important;
  gap: [GAP] !important;
}
.footer__content-top .footer__blocks-wrapper .grid__item,
.footer__content-top .footer__blocks-wrapper .footer-block {
  width: 100% !important;
  max-width: 100% !important;
  flex: none !important;
  padding: 0 !important;
}
```

---

## Phase 7 — Output Format

Every response that produces code must follow this structure:

### 1. File path + action
```
📁 assets/base.css — APPEND at end
📁 sections/my-new-section.liquid — CREATE new file
📁 templates/index.json — UPDATE sections object and order array
```

### 2. Complete code block
- New files: always complete, never partial
- Existing files: show the exact block to append or the exact lines to replace
- Never say "add the rest of your existing code here"

### 3. Theme Editor steps (if any)
List any required actions in Shopify Admin or Customize after the code change.

### 4. What changed and why
Brief summary — one line per change.

---

## 🔴 Absolute Rules — Never Violate

| Rule | Detail |
|---|---|
| Never remove `{% render 'content_for_header' %}` | Required in `layout/theme.liquid` `<head>` |
| Never remove `{{ content_for_layout }}` | Required in `layout/theme.liquid` `<body>` |
| Never rewrite `{% form 'product' %}` or `{% form 'cart' %}` | Breaks purchasing |
| Never rewrite cart/product JS | `cart.js`, `product-form.js`, `global.js` |
| Never modify checkout | Shopify-hosted since Aug 2024 |
| Never delete `config/settings_schema.json` | Breaks Theme Editor |
| Never delete `templates/product.json` or `templates/cart.json` | Breaks commerce |
| Never paste raw static HTML | All content must use Liquid variables |
| Never hardcode font names in CSS | Use `var(--font-body-family)` / `var(--font-heading-family)` |
| Never use icon font classes | Convert to inline SVG |

---

---

# Reference A — Dawn Native Sections: Class Names & Safe Override Points

## image-banner (Hero)

Key classes:
- `.image-banner` — section wrapper
- `.image-banner__media` — image container
- `.image-banner__content` — text overlay container
- `.image-banner__heading` — main heading (h2 by default)
- `.image-banner__subheading` — eyebrow / label text
- `.image-banner__buttons` — CTA button group

Safe insertion points:
- Between `.image-banner__subheading` and heading: add decorative dividers
- After `.image-banner__buttons`: add additional static links

Never touch:
- The `{% if section.settings.image %}` block — controls image rendering

---

## featured-collection (Product Grid)

Key classes:
- `.featured-collection` — section wrapper
- `.featured-collection__title` — section heading
- `.card` — individual product card
- `.card__heading` — product title
- `.card__vendor` — vendor/subtitle text
- `.card__information` — price + meta wrapper
- `.price__regular`, `.price-item` — price display

Safe CSS overrides: all of the above
Never touch: Add-to-cart form inside `.card__content`

---

## email-signup (Newsletter)

Key classes:
- `.newsletter` — section wrapper
- `.newsletter__heading` — heading
- `.newsletter-form` — form wrapper (contains the `{% form 'customer' %}`)
- `.newsletter-form__button` — submit button
- `.field__input` — email input
- `.field__label` — floating label

Safe CSS overrides: all visual classes above
**Never touch**: `{% form 'customer', id: 'ContactFooter' %}` and everything inside

---

## multicolumn (Icon/Feature Columns)

Key classes:
- `.multicolumn` — section wrapper
- `.multicolumn__title` — section heading
- `.multicolumn-list` — columns container
- `.multicolumn-list__item` — individual column
- `.multicolumn-card` — card wrapper
- `.multicolumn-card__info` — text content

Default layout: vertical stacking within each column, horizontal columns
If the design needs horizontal icon+text layout within each column, override with flexbox.

---

## footer (sections/footer.liquid)

Key classes:
- `.footer` — root element
- `.footer__content-top` — blocks area (columns)
- `.footer__blocks-wrapper` — grid container for columns
  Dawn assigns one of: `grid--2-col`, `grid--4-col-tablet`, `grid--3-col-tablet`, `grid--4-col-desktop`
  based on block count — override ALL of them
- `.footer-block` — individual column wrapper (also has `.grid__item`)
- `.footer-block__heading` — column title (rendered as `<h2>`)
- `.footer-block__details-content` — column content (links, text)
- `.footer__content-bottom` — copyright + payment row
- `.footer__copyright` — copyright text
- `.list-social` — social icons list (NOT `.footer__list-social`)
- `.list-social__link` — individual social icon link
- `.list-payment` — payment icons list

Never touch: see Phase 6 above

---

## header (sections/header.liquid)

Key classes:
- `.header-wrapper` — outermost wrapper (sticky target)
- `.header` — inner header element
  Gets modifier: `header--[logo_position]` e.g. `header--middle-center`
- `.header__heading` — logo/brand name container
- `.header__heading-link` — anchor wrapping logo
- `.header__heading-logo` — logo `<img>` (if uploaded)
- `.header__inline-menu` — desktop nav container
- `.header__menu-item` — nav item wrapper
- `.list-menu--inline` — horizontal nav list
- `.header__icons` — right-side icons (search, account, cart)
- `.header__icon--cart` — cart icon (contains `cart-count-bubble`)
- `.header__icon--account` — account icon

Never touch: cart icon, account link, search modal, sticky JS

---

---

# Reference B — Liquid Patterns: Reusable Snippets

## 1. Full-width image section

```liquid
<section style="position:relative; height:[HEIGHT]; overflow:hidden;">
  {%- if section.settings.image -%}
    <div style="position:absolute; inset:0;">
      {{ section.settings.image | image_url: width: 2000 | image_tag:
          style: 'width:100%; height:100%; object-fit:cover;',
          preload: true }}
    </div>
  {%- endif -%}
  {%- if section.settings.overlay_opacity > 0 -%}
    <div style="position:absolute; inset:0; background:rgba(0,0,0,{{ section.settings.overlay_opacity | divided_by: 100.0 }});"></div>
  {%- endif -%}
  <div style="position:relative; z-index:2; height:100%; display:flex; flex-direction:column;
              align-items:center; justify-content:center; text-align:center; color:white; padding:2rem;">
    <h1>{{ section.settings.heading }}</h1>
    <p>{{ section.settings.subheading }}</p>
    {%- if section.settings.button_label != blank -%}
      <a href="{{ section.settings.button_url }}" class="btn-primary">
        {{ section.settings.button_label }}
      </a>
    {%- endif -%}
  </div>
</section>
```

Schema settings needed: `image_picker`, `range` (overlay_opacity 0–100), `text` (heading), `text` (subheading), `text` (button_label), `url` (button_url)

---

## 2. Two-column layout (image + text)

```liquid
<section style="padding:[PADDING];">
  <div style="max-width:90rem; margin:0 auto; display:grid;
              grid-template-columns:1fr 1fr; gap:[GAP]; align-items:center;">

    {%- if section.settings.image_position == 'left' -%}
      <div style="height:[IMG_HEIGHT]; overflow:hidden; border-radius:[RADIUS];">
        {%- if section.settings.image -%}
          {{ section.settings.image | image_url: width: 900 | image_tag:
              style: 'width:100%; height:100%; object-fit:cover;' }}
        {%- endif -%}
      </div>
    {%- endif -%}

    <div>
      {%- if section.settings.label != blank -%}
        <span style="/* eyebrow style */">{{ section.settings.label }}</span>
      {%- endif -%}
      <h2>{{ section.settings.heading }}</h2>
      <p>{{ section.settings.text }}</p>
      {%- if section.settings.btn1_label != blank -%}
        <a href="{{ section.settings.btn1_url }}" class="btn-primary">{{ section.settings.btn1_label }}</a>
      {%- endif -%}
      {%- if section.settings.btn2_label != blank -%}
        <a href="{{ section.settings.btn2_url }}" class="btn-outline">{{ section.settings.btn2_label }}</a>
      {%- endif -%}
    </div>

    {%- if section.settings.image_position == 'right' -%}
      <div style="height:[IMG_HEIGHT]; overflow:hidden; border-radius:[RADIUS];">
        {%- if section.settings.image -%}
          {{ section.settings.image | image_url: width: 900 | image_tag:
              style: 'width:100%; height:100%; object-fit:cover;' }}
        {%- endif -%}
      </div>
    {%- endif -%}

  </div>
</section>
```

Schema settings: `image_picker`, `select` (image_position: left/right), `text` (label), `text` (heading), `textarea` (text), `text`×2 (btn labels), `url`×2 (btn links)

---

## 3. Card grid with blocks

```liquid
<section style="padding:[PADDING]; max-width:90rem; margin:0 auto;">
  {%- if section.settings.heading != blank -%}
    <div style="text-align:center; margin-bottom:[HEADING_MB];">
      <h2>{{ section.settings.heading }}</h2>
      {%- if section.settings.subheading != blank -%}
        <p>{{ section.settings.subheading }}</p>
      {%- endif -%}
    </div>
  {%- endif -%}

  <div style="display:grid; grid-template-columns:repeat([N], 1fr); gap:[GAP];">
    {%- for block in section.blocks -%}
      <div {{ block.shopify_attributes }}>

        <!-- Fixed-height image wrapper — prevents unequal heights -->
        <div style="width:100%; height:[IMG_H]; overflow:hidden; border-radius:[RADIUS];">
          {%- if block.settings.image -%}
            {{ block.settings.image | image_url: width: [WIDTH] | image_tag:
                style: 'width:100%; height:100%; object-fit:cover; display:block;' }}
          {%- endif -%}
        </div>

        <div style="padding:[CARD_PADDING];">
          {%- if block.settings.label != blank -%}
            <p style="/* label style */">{{ block.settings.label }}</p>
          {%- endif -%}
          <h3>{{ block.settings.title }}</h3>
          {%- if block.settings.text != blank -%}
            <p>{{ block.settings.text }}</p>
          {%- endif -%}
          {%- if block.settings.link != blank and block.settings.link_label != blank -%}
            <a href="{{ block.settings.link }}">{{ block.settings.link_label }}</a>
          {%- endif -%}
        </div>

      </div>
    {%- endfor -%}
  </div>
</section>
```

Schema blocks: `image_picker`, `text` (label), `text` (title), `textarea` (text), `url` (link), `text` (link_label)

---

## 4. Dark editorial section

```liquid
<section style="background:[DARK_BG]; color:white; overflow:hidden;">
  <div style="display:grid; grid-template-columns:1fr 1fr; align-items:center;">

    <div style="padding:clamp(3rem, 8vw, 6rem);">
      {%- if section.settings.label != blank -%}
        <span style="color:[PRIMARY]; text-transform:uppercase; letter-spacing:0.3em;
                     font-size:0.7rem; font-weight:700;">
          {{ section.settings.label }}
        </span>
      {%- endif -%}
      <h2 style="font-size:clamp(2rem,4vw,3.5rem); line-height:1.2; margin:1.5rem 0;">
        {{ section.settings.heading }}
      </h2>
      <p style="opacity:0.7; font-weight:300; line-height:1.8;">
        {{ section.settings.text }}
      </p>
      {%- if section.settings.button_label != blank -%}
        <a href="{{ section.settings.button_url }}"
           style="display:inline-flex; align-items:center; gap:1rem; color:white;
                  text-decoration:none; text-transform:uppercase; letter-spacing:0.15em;
                  font-size:0.75rem; margin-top:2rem;">
          {{ section.settings.button_label }} →
        </a>
      {%- endif -%}
    </div>

    <div style="height:[IMG_HEIGHT]; overflow:hidden;">
      {%- if section.settings.image -%}
        {{ section.settings.image | image_url: width: 1000 | image_tag:
            style: 'width:100%; height:100%; object-fit:cover; [OPTIONAL FILTERS]' }}
      {%- endif -%}
    </div>

  </div>
</section>
```

Optional image filters: `filter:grayscale(100%) brightness(0.75)` for dramatic b&w look.

---

## 5. Icon/trust bar (SVG icons)

Pattern for a configurable icon bar. Use `select` schema field + `{% case %}` to map
icon names to SVG paths. Never use icon font classes.

```liquid
<section style="padding:[PADDING]; background:[BG];">
  <div style="max-width:90rem; margin:0 auto;
              display:grid; grid-template-columns:repeat([N],1fr);
              gap:[GAP]; text-align:center;">
    {%- for block in section.blocks -%}
      <div style="display:flex; flex-direction:column; align-items:center;"
           {{ block.shopify_attributes }}>

        <div style="margin-bottom:1rem;">
          {%- case block.settings.icon -%}
            {%- when 'icon_name_1' -%}
              <svg xmlns="http://www.w3.org/2000/svg" height="32" width="32"
                   viewBox="..." fill="[PRIMARY COLOR]">
                <path d="..."/>
              </svg>
            {%- when 'icon_name_2' -%}
              <svg ...>...</svg>
          {%- endcase -%}
        </div>

        <h4 style="font-size:0.7rem; text-transform:uppercase; letter-spacing:0.2em;
                   font-weight:700; font-style:normal !important;">
          {{ block.settings.title }}
        </h4>
        <p style="font-size:0.875rem; font-weight:300; margin:0;">
          {{ block.settings.text }}
        </p>
      </div>
    {%- endfor -%}
  </div>
</section>
```

Use `"type": "select"` in schema with predefined icon options — prevents merchant typos.

---

## 6. Schema field reference

```json
{ "type": "text",         "id": "heading",   "label": "Heading",     "default": "..." }
{ "type": "textarea",     "id": "text",      "label": "Description", "default": "..." }
{ "type": "richtext",     "id": "body",      "label": "Body Text" }
{ "type": "inline_richtext", "id": "heading", "label": "Heading" }
{ "type": "image_picker", "id": "image",     "label": "Image" }
{ "type": "url",          "id": "link",      "label": "Link" }
{ "type": "checkbox",     "id": "show_x",    "label": "Show X",      "default": true }
{ "type": "range",        "id": "opacity",   "label": "Overlay",     "min": 0, "max": 100, "step": 5, "unit": "%", "default": 30 }
{ "type": "select",       "id": "position",  "label": "Position",
  "options": [{"value": "left", "label": "Left"}, {"value": "right", "label": "Right"}],
  "default": "left" }
{ "type": "color",        "id": "bg_color",  "label": "Background",  "default": "#ffffff" }
{ "type": "header",       "content": "Section title — groups settings visually" }
{ "type": "paragraph",    "content": "Helper text shown in the editor" }
```

### Presets (required for Add Section menu):
```json
"presets": [{ "name": "My Section Name" }]
```

### Block structure template:
```json
"blocks": [
  {
    "type": "item",
    "name": "Item",
    "settings": [
      { "type": "image_picker", "id": "image", "label": "Image" },
      { "type": "text",         "id": "title", "label": "Title" },
      { "type": "textarea",     "id": "text",  "label": "Description" },
      { "type": "url",          "id": "link",  "label": "Link" }
    ]
  }
]
```
