# BEM Patterns Reference for SCSS Components

This document is a concise BEM playbook for generating maintainable SCSS and semantic HTML in Rails applications.

## Why This Exists

Use this file as the ruleset behind `component-naming.md`.

It is intended for:

- Agentic AI SKILLS that need to name new components consistently
- refactors from Tailwind or utility-heavy markup into semantic SCSS
- Rails teams using ERB/HAML, ViewComponent, and Stimulus
- shared guidance for block, element, and modifier decisions

---

## 1) BEM Definitions

### Block

A standalone UI object that is meaningful on its own.

Examples:

- `button`
- `card`
- `alert`
- `restaurant-card`

### Element

A meaningful part of a block that only makes sense inside that block.

Examples:

- `card__header`
- `alert__message`
- `button__icon`

### Modifier

A flag that changes a block or element’s appearance, behavior, or state.

Examples:

- `button--primary`
- `alert--success`
- `button__icon--leading`

---

## 2) Canonical Naming Format

### Block

```text
.block
```

### Element

```text
.block__element
```

### Modifier

```text
.block--modifier
.block__element--modifier
```

### Preferred Character Rules

- lowercase letters only
- use hyphens for multi-word names
- do not camelCase class names
- do not use underscores except in the BEM `__` element separator
- do not use single-dash pseudo-BEM forms like `.block-element`

---

## 3) Rules to Follow

### Rule A: Style by Class, Not Tag or ID

Good:

```scss
.card { }
.card__title { }
```

Bad:

```scss
div.card { }
#card { }
.card h3 { }
```

### Rule B: Elements Belong to Their Block Semantically

Good:

```scss
.card__header { }
.card__body { }
```

Bad:

```scss
.header-inside-card { }
.card .header { }
```

### Rule C: Modifiers Do Not Replace the Base Class

Good:

```html
<button class="button button--primary">Save</button>
```

Bad:

```html
<button class="button--primary">Save</button>
```

### Rule D: Prefer Semantic Names Over Visual Names

Good:

- `badge--pending`
- `alert--error`
- `button--secondary`

Bad:

- `badge--yellow`
- `alert--red`
- `button--gray`

### Rule E: Do Not Encode DOM Depth into Names

Good:

- `card__footer`

Bad:

- `page-content-card-footer`

### Rule F: Do Not Bind Component Styling to Page Context

Good:

```scss
.alert { }
```

Bad:

```scss
.dashboard .alert { }
.sidebar .alert { }
```

If a component really changes in a certain context, prefer a modifier or a higher-level layout wrapper.

---

## 4) How to Decide Between Block vs Element

Choose a **block** when the thing:

- can be reused outside the current component
- has its own API or identity
- may gain variants of its own
- would still make sense if moved elsewhere

Choose an **element** when the thing:

- only exists as part of the parent component
- has no independent identity
- should not be reused outside the block

### Quick Test

If you can imagine using it elsewhere without the parent, make it a block.

Examples:

- `button` is a block
- `card__title` is an element
- `checkbox` is usually a block
- `field__label` is an element

---

## 5) How to Decide Between Modifier vs New Block

Choose a **modifier** when the change:

- is still the same component conceptually
- changes appearance or state, not identity
- shares most structure with the base block

Choose a **new block** when the component:

- has distinct meaning in the product domain
- needs different structure, semantics, or API
- is likely to evolve independently

### Examples

Use a modifier:

- `card card--hoverable`
- `button button--danger`

Use a new block:

- `restaurant-card`
- `project-summary`
- `daily-report-entry`

---

## 6) SCSS Nesting Guidelines

### Preferred

```scss
.card {
  &__header { }
  &__body { }
  &--interactive { }
}
```

### Avoid Deep Nesting

Bad:

```scss
.card {
  .card__header {
    .card__title {
      span {
        strong { }
      }
    }
  }
}
```

### Practical Rule

- nest only enough to express the block, its elements, and its modifiers
- avoid selector chains that depend on exact DOM structure
- prefer flat, readable selectors over “smart” nested specificity

---

## 7) State Conventions

There are two acceptable patterns. Pick one per codebase and stay consistent.

### Option A: BEM State as Modifier

```html
<button class="button button--loading">Saving…</button>
```

Best when the state is part of the component’s public styling API.

### Option B: Separate State Class

```html
<button class="button is-loading">Saving…</button>
```

Best when the codebase already has global state classes.

### Preferred Default for This Library

Use **BEM modifiers for component-owned states**:

- `button--loading`
- `field--invalid`
- `alert--dismissible`

Use global state classes only when a project already depends on them heavily.

---

## 8) JavaScript / Stimulus Guidance

Do not use CSS class names as your primary JavaScript API unless the class is truly part of the component contract.

Prefer:

```erb
<div class="alert alert--dismissible" data-controller="dismissible">
  <button class="alert__dismiss" data-action="dismissible#dismiss">×</button>
</div>
```

Over:

```erb
<div class="alert js-dismissible-alert">
  <button class="alert__dismiss js-dismiss-alert">×</button>
</div>
```

### Guidance

- BEM classes describe structure and appearance
- `data-controller`, `data-action`, and `data-*` describe behavior
- do not add JS-only classes when a `data-*` API is cleaner

---

## 9) Rails / ViewComponent Guidance

### Prefer Semantic Component APIs

Good:

```ruby
ButtonComponent.new(variant: :primary, size: :small)
```

Avoid:

```ruby
ButtonComponent.new(classes: "bg-blue-600 px-4 py-2 rounded-md")
```

### Let Components Assemble Their Own BEM Classes

Good:

```ruby
def classes
  ["button", "button--#{variant}", size_modifier].compact.join(" ")
end
```

### Keep Markup Predictable

A component should render stable element names so styles and tests stay reliable.

---

## 10) File and Folder Patterns for SCSS

### Suggested Structure

```text
app/assets/stylesheets/
  abstracts/
    _tokens.scss
    _mixins.scss
  components/
    _button.scss
    _field.scss
    _card.scss
    _alert.scss
    _badge.scss
```

### Preferred Pattern

- one block per partial when practical
- put related tiny primitives together only if they are always used together
- keep tokens and mixins separate from component rules

---

## 11) Anti-Patterns to Avoid

### Fake BEM

Bad:

- `.button_primary`
- `.button-primary-large`
- `.button__icon_primary`

### Utility Leakage

Bad:

```erb
<div class="card mt-4 text-blue-600 flex items-center">
```

If spacing or layout is part of the page layout and not the card itself, use a layout wrapper instead.

### Contextual Overrides

Bad:

```scss
.modal .button { }
.sidebar .button { }
```

Prefer:

```scss
.button--inverted { }
```

or a dedicated parent block whose elements include the button placement.

### Over-Elementizing

Bad:

- `card__paragraph`
- `card__span`
- `card__strong`

Name meaningful UI parts, not raw HTML tags.

### Over-Modifiering

Bad:

- `button--blue`
- `button--rounded`
- `button--padding-large`

These become utility classes wearing a BEM costume. Prefer semantic variants.

---

## 12) Decision Checklist for New Components

Before naming a new class, ask:

1. Is this a standalone thing? If yes, make it a block.
2. Is this only meaningful inside a block? If yes, make it an element.
3. Is this just a variant or state? If yes, make it a modifier.
4. Is the name semantic and product-facing rather than presentational?
5. Can this class survive markup changes without being renamed?
6. Am I leaking layout or utility concerns into the component API?

---

## 13) Recommended Defaults for Agentic AI SKILLS

When generating new SCSS or markup, default to these rules:

- use one semantic block per reusable component
- use elements only for meaningful internal parts
- use modifiers for variants and states
- use domain names when a generic block is no longer descriptive enough
- keep behavior in `data-*` attributes, not JS helper classes
- avoid tag selectors, IDs, and page-context selectors
- avoid utility-class output unless explicitly requested

---

## 14) Mini Reference Examples

### Button

```html
<button class="button button--primary">
  <span class="button__text">Save</span>
</button>
```

### Alert

```html
<div class="alert alert--error" role="alert">
  <h4 class="alert__title">Error</h4>
  <p class="alert__message">Something went wrong.</p>
</div>
```

### Card

```html
<article class="card card--hoverable">
  <header class="card__header">
    <h3 class="card__title">Card title</h3>
  </header>
  <div class="card__body">...</div>
</article>
```

### Field

```html
<div class="field field--invalid">
  <label class="field__label">Name</label>
  <input class="field__input" />
  <p class="field__error">Name is required.</p>
</div>
```

---

## 15) One-Line Summary

Name components by meaning, structure them as blocks with elements, and express variants or state through modifiers instead of utility classes or context-dependent selectors.
