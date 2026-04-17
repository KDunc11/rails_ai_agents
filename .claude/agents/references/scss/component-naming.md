# Component Naming Reference (SCSS + BEM)

This file converts the Tailwind component patterns from the source reference into an SCSS-first, BEM-based component library.

## Purpose

Use this document when:

- creating or refactoring UI components from utility classes into semantic SCSS
- naming new components consistently across ERB, ViewComponent, Stimulus, and SCSS
- deciding whether something should be a block, element, or modifier
- building reusable Rails components without depending on Tailwind class composition

## Scope

These conventions are based on the source component set:

- buttons
- form fields
- cards
- alerts and notifications
- badges
- loading states
- Turbo-related UI containers
- extraction into reusable components

## Core Naming Rules

- Use **blocks** for standalone UI objects: `.button`, `.card`, `.alert`
- Use **elements** for parts of a block: `.card__header`, `.alert__message`
- Use **modifiers** for variants and states: `.button--primary`, `.alert--success`
- Keep JavaScript hooks separate from styling when possible. Prefer `data-*` for behavior and BEM classes for styling.
- Avoid encoding layout-only concerns into component names unless the layout is part of the component contract.
- Avoid utility-style names like `.mt-4`, `.flex-center`, or `.text-blue-600` in component markup.

---

## 1) Buttons

### Block

- `.button`

### Elements

- `.button__text`
- `.button__icon`
- `.button__spinner`

### Modifiers

- `.button--primary`
- `.button--secondary`
- `.button--danger`
- `.button--icon`
- `.button--loading`
- `.button--disabled`
- `.button--full-width`
- `.button--small`
- `.button--large`

### Recommended HTML / ERB

```erb
<%= button_to "Create", create_path, class: "button button--primary" %>

<%= link_to back_path, class: "button button--secondary" do %>
  <span class="button__text">Cancel</span>
<% end %>

<%= button_to delete_path,
    method: :delete,
    data: { turbo_confirm: "Are you sure?" },
    class: "button button--danger" do %>
  <span class="button__text">Delete</span>
<% end %>

<button type="button" class="button button--icon" aria-label="Close">
  <svg class="button__icon" aria-hidden="true" viewBox="0 0 20 20">
    <path d="M6.28 5.22a.75.75 0 00-1.06 1.06L8.94 10l-3.72 3.72a.75.75 0 101.06 1.06L10 11.06l3.72 3.72a.75.75 0 101.06-1.06L11.06 10l3.72-3.72a.75.75 0 00-1.06-1.06L10 8.94 6.28 5.22z" />
  </svg>
</button>
```

### Recommended SCSS

```scss
.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  min-height: 2.5rem;
  padding: 0.625rem 1rem;
  border: 1px solid transparent;
  border-radius: 0.375rem;
  font-weight: 600;
  line-height: 1;
  text-decoration: none;
  cursor: pointer;
  transition: background-color 0.2s ease, border-color 0.2s ease, color 0.2s ease, box-shadow 0.2s ease;

  &:focus-visible {
    outline: 0;
    box-shadow: 0 0 0 2px rgba(59, 130, 246, 0.35);
  }

  &__text {
    display: inline-flex;
    align-items: center;
  }

  &__icon,
  &__spinner {
    flex: 0 0 auto;
    width: 1rem;
    height: 1rem;
  }

  &__spinner {
    display: none;
  }

  &--primary {
    background: #2563eb;
    color: #fff;

    &:hover { background: #1d4ed8; }
    &:active { background: #1e40af; }
  }

  &--secondary {
    background: #f3f4f6;
    border-color: #d1d5db;
    color: #374151;

    &:hover { background: #e5e7eb; }
    &:active { background: #d1d5db; }
  }

  &--danger {
    background: #dc2626;
    color: #fff;

    &:hover { background: #b91c1c; }
    &:active { background: #991b1b; }
  }

  &--icon {
    width: 2.5rem;
    min-width: 2.5rem;
    padding: 0;
    border-radius: 9999px;
  }

  &--loading {
    pointer-events: none;

    .button__spinner {
      display: inline-flex;
    }
  }

  &--disabled,
  &:disabled,
  &[aria-disabled="true"] {
    opacity: 0.6;
    cursor: not-allowed;
  }

  &--full-width {
    width: 100%;
  }

  &--small {
    min-height: 2rem;
    padding: 0.5rem 0.75rem;
    font-size: 0.875rem;
  }

  &--large {
    min-height: 3rem;
    padding: 0.75rem 1.25rem;
    font-size: 1rem;
  }
}
```

### Notes

- Use modifiers for visual variants, not separate blocks like `.button-primary`.
- Use `.button--loading` rather than a generic `.is-loading` unless the project has a shared state convention.
- Prefer the same `.button` block for `button_to`, `link_to`, and native `<button>` elements.

---

## 2) Form Fields

### Blocks

- `.field`
- `.checkbox`

### `field` Elements

- `.field__label`
- `.field__control`
- `.field__input`
- `.field__select`
- `.field__textarea`
- `.field__hint`
- `.field__error`

### `field` Modifiers

- `.field--required`
- `.field--invalid`
- `.field--disabled`
- `.field--select`
- `.field--textarea`

### `checkbox` Elements

- `.checkbox__input`
- `.checkbox__label`
- `.checkbox__text`

### `checkbox` Modifiers

- `.checkbox--disabled`
- `.checkbox--invalid`

### Recommended HTML / ERB

```erb
<div class="field">
  <%= f.label :name, class: "field__label" %>
  <div class="field__control">
    <%= f.text_field :name, class: "field__input", placeholder: "Enter name..." %>
  </div>
</div>

<div class="field field--select">
  <%= f.label :status, class: "field__label" %>
  <div class="field__control">
    <%= f.select :status, ["Active", "Inactive"], { include_blank: "Select status" }, class: "field__select" %>
  </div>
</div>

<div class="field field--textarea">
  <%= f.label :description, class: "field__label" %>
  <div class="field__control">
    <%= f.text_area :description, rows: 4, class: "field__textarea", placeholder: "Enter description..." %>
  </div>
</div>

<div class="checkbox">
  <%= f.check_box :terms, class: "checkbox__input" %>
  <%= f.label :terms, class: "checkbox__label" do %>
    <span class="checkbox__text">I agree to the terms and conditions</span>
  <% end %>
</div>
```

### Recommended SCSS

```scss
.field {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;

  &__label {
    display: block;
    font-size: 0.875rem;
    font-weight: 500;
    color: #374151;
  }

  &__control {
    display: block;
  }

  &__input,
  &__select,
  &__textarea {
    width: 100%;
    padding: 0.625rem 0.75rem;
    border: 1px solid #d1d5db;
    border-radius: 0.375rem;
    background: #fff;
    color: #111827;
    transition: border-color 0.2s ease, box-shadow 0.2s ease;

    &::placeholder {
      color: #9ca3af;
    }

    &:focus {
      outline: 0;
      border-color: #3b82f6;
      box-shadow: 0 0 0 2px rgba(59, 130, 246, 0.2);
    }
  }

  &__textarea {
    resize: vertical;
    min-height: 6rem;
  }

  &__hint {
    font-size: 0.875rem;
    color: #6b7280;
  }

  &__error {
    font-size: 0.875rem;
    color: #b91c1c;
  }

  &--invalid {
    .field__input,
    .field__select,
    .field__textarea {
      border-color: #dc2626;
    }
  }

  &--disabled {
    opacity: 0.7;

    .field__input,
    .field__select,
    .field__textarea {
      background: #f9fafb;
      cursor: not-allowed;
    }
  }
}

.checkbox {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;

  &__input {
    width: 1rem;
    height: 1rem;
    border-radius: 0.25rem;
  }

  &__label {
    display: inline-flex;
    align-items: center;
    cursor: pointer;
  }

  &__text {
    font-size: 0.875rem;
    color: #374151;
  }

  &--disabled {
    opacity: 0.7;
  }
}
```

### Notes

- Use `field` for label + control wrappers.
- Keep checkbox as its own block because it behaves differently from text/select/textarea controls.
- Do not name inputs by tag type alone unless the tag distinction matters in the API.

---

## 3) Cards

### Block

- `.card`

### Elements

- `.card__header`
- `.card__title`
- `.card__body`
- `.card__footer`
- `.card__actions`
- `.card__description`
- `.card__link`

### Modifiers

- `.card--interactive`
- `.card--hoverable`
- `.card--compact`
- `.card--spacious`

### Recommended HTML / ERB

```erb
<div class="card">
  <div class="card__body">
    <h3 class="card__title">Card Title</h3>
    <p class="card__description">Card content goes here.</p>
  </div>
</div>

<div class="card">
  <div class="card__header">
    <h3 class="card__title">Card Header</h3>
  </div>

  <div class="card__body">
    <p class="card__description">Card body content.</p>
  </div>

  <div class="card__footer">
    <div class="card__actions">
      <%= link_to "Cancel", "#", class: "button button--secondary button--small" %>
      <%= link_to "Save", "#", class: "button button--primary button--small" %>
    </div>
  </div>
</div>

<%= link_to item_path(@item), class: "card card--interactive card--hoverable" do %>
  <div class="card__body">
    <h3 class="card__title"><%= @item.title %></h3>
    <p class="card__description"><%= @item.description %></p>
  </div>
<% end %>
```

### Recommended SCSS

```scss
.card {
  display: block;
  overflow: hidden;
  border-radius: 0.5rem;
  background: #fff;
  box-shadow: 0 4px 12px rgba(17, 24, 39, 0.08);
  color: inherit;
  text-decoration: none;

  &__header,
  &__footer {
    padding: 1rem 1.5rem;
    background: #f9fafb;
  }

  &__header {
    border-bottom: 1px solid #e5e7eb;
  }

  &__body {
    padding: 1.5rem;
  }

  &__footer {
    border-top: 1px solid #e5e7eb;
  }

  &__title {
    margin: 0;
    font-size: 1.125rem;
    font-weight: 600;
    color: #1f2937;
  }

  &__description {
    margin-top: 0.5rem;
    color: #4b5563;
  }

  &__actions {
    display: flex;
    justify-content: flex-end;
    gap: 0.75rem;
  }

  &--interactive {
    transition: transform 0.3s ease, box-shadow 0.3s ease;
  }

  &--hoverable:hover {
    transform: translateY(-0.25rem);
    box-shadow: 0 10px 24px rgba(17, 24, 39, 0.14);
  }

  &--compact {
    .card__header,
    .card__body,
    .card__footer {
      padding: 1rem;
    }
  }
}
```

### Notes

- A clickable card can still use `.card` as the block when the whole component is the interactive target.
- Use `.card--interactive` or `.card--hoverable` instead of creating a separate `list-card` block unless behavior diverges significantly.

---

## 4) Alerts and Notifications

### Blocks

- `.alert`
- `.notification-stack`

### `alert` Elements

- `.alert__inner`
- `.alert__icon`
- `.alert__content`
- `.alert__title`
- `.alert__message`
- `.alert__dismiss`

### `alert` Modifiers

- `.alert--success`
- `.alert--error`
- `.alert--info`
- `.alert--warning`
- `.alert--dismissible`

### `notification-stack` Elements

- `.notification-stack__item`

### Recommended HTML / ERB

```erb
<div class="alert alert--success" role="alert">
  <div class="alert__inner">
    <svg class="alert__icon" aria-hidden="true" viewBox="0 0 20 20"></svg>
    <div class="alert__content">
      <h4 class="alert__title">Success!</h4>
      <p class="alert__message">Your changes have been saved.</p>
    </div>
  </div>
</div>

<div class="alert alert--error" role="alert">
  <div class="alert__inner">
    <svg class="alert__icon" aria-hidden="true" viewBox="0 0 20 20"></svg>
    <div class="alert__content">
      <h4 class="alert__title">Error</h4>
      <p class="alert__message">Something went wrong. Please try again.</p>
    </div>
  </div>
</div>

<div class="alert alert--info alert--dismissible"
     data-controller="dismissible"
     role="alert">
  <div class="alert__inner">
    <div class="alert__content">
      <p class="alert__message">This is an informational message.</p>
    </div>
    <button type="button"
            class="alert__dismiss"
            data-action="dismissible#dismiss"
            aria-label="Dismiss">
      <span aria-hidden="true">&times;</span>
    </button>
  </div>
</div>

<div id="notifications" class="notification-stack">
  <%# Turbo Streams append .notification-stack__item nodes here %>
</div>
```

### Recommended SCSS

```scss
.alert {
  border: 1px solid;
  border-radius: 0.375rem;
  padding: 1rem;

  &__inner {
    display: flex;
    align-items: flex-start;
    justify-content: space-between;
    gap: 0.75rem;
  }

  &__icon {
    flex: 0 0 auto;
    width: 1.25rem;
    height: 1.25rem;
  }

  &__content {
    flex: 1 1 auto;
  }

  &__title {
    margin: 0;
    font-size: 0.875rem;
    font-weight: 600;
  }

  &__message {
    margin-top: 0.25rem;
    font-size: 0.875rem;
  }

  &__dismiss {
    flex: 0 0 auto;
    border: 0;
    background: transparent;
    cursor: pointer;
  }

  &--success {
    background: #f0fdf4;
    border-color: #bbf7d0;
    color: #166534;
  }

  &--error {
    background: #fef2f2;
    border-color: #fecaca;
    color: #991b1b;
  }

  &--info {
    background: #eff6ff;
    border-color: #bfdbfe;
    color: #1d4ed8;
  }

  &--warning {
    background: #fffbeb;
    border-color: #fde68a;
    color: #92400e;
  }
}

.notification-stack {
  position: fixed;
  top: 1rem;
  right: 1rem;
  z-index: 50;
  width: 20rem;
  pointer-events: none;

  &__item + &__item {
    margin-top: 0.5rem;
  }
}
```

### Notes

- Use `notification-stack` for the fixed Turbo Stream destination.
- Treat each appended alert as either `.notification-stack__item > .alert` or as a standalone `.alert` within the stack.

---

## 5) Badges

### Block

- `.badge`

### Modifiers

- `.badge--active`
- `.badge--inactive`
- `.badge--deleted`
- `.badge--pending`
- `.badge--small`
- `.badge--large`

### Recommended HTML / ERB

```erb
<span class="badge badge--active">Active</span>
<span class="badge badge--inactive">Inactive</span>
<span class="badge badge--deleted">Deleted</span>
<span class="badge badge--pending">Pending</span>
```

### Recommended SCSS

```scss
.badge {
  display: inline-flex;
  align-items: center;
  padding: 0.125rem 0.625rem;
  border-radius: 9999px;
  font-size: 0.75rem;
  font-weight: 500;
  line-height: 1.5;

  &--active {
    background: #dcfce7;
    color: #166534;
  }

  &--inactive {
    background: #f3f4f6;
    color: #1f2937;
  }

  &--deleted {
    background: #fee2e2;
    color: #991b1b;
  }

  &--pending {
    background: #fef3c7;
    color: #92400e;
  }
}
```

### Notes

- Badges should usually remain a very small, single-purpose block.
- Prefer semantic status modifiers over presentational modifiers like `.badge--green`.

---

## 6) Loading States

### Blocks

- `.spinner`
- `.skeleton`

### `spinner` Modifiers

- `.spinner--small`
- `.spinner--medium`
- `.spinner--large`
- `.spinner--primary`

### `skeleton` Elements

- `.skeleton__line`

### `skeleton` Modifiers

- `.skeleton--text`
- `.skeleton--card`

### Recommended HTML / ERB

```erb
<div class="spinner spinner--medium spinner--primary" aria-hidden="true"></div>

<div class="skeleton skeleton--text" aria-hidden="true">
  <div class="skeleton__line"></div>
  <div class="skeleton__line skeleton__line--half"></div>
  <div class="skeleton__line skeleton__line--wide"></div>
</div>

<button class="button button--primary button--loading"
        data-controller="loading"
        data-action="loading#submit"
        data-loading-text-value="Saving...">
  <span class="button__text" data-loading-target="text">Save</span>
  <svg class="button__spinner" data-loading-target="spinner" viewBox="0 0 24 24"></svg>
</button>
```

### Recommended SCSS

```scss
.spinner {
  display: inline-block;
  border: 2px solid rgba(37, 99, 235, 0.2);
  border-top-color: #2563eb;
  border-radius: 50%;
  animation: spin 0.8s linear infinite;

  &--small {
    width: 1rem;
    height: 1rem;
  }

  &--medium {
    width: 2rem;
    height: 2rem;
  }

  &--large {
    width: 3rem;
    height: 3rem;
  }
}

.skeleton {
  display: flex;
  flex-direction: column;
  gap: 1rem;

  &__line {
    height: 1rem;
    border-radius: 0.25rem;
    background: #e5e7eb;
  }

  &__line--half {
    width: 50%;
  }

  &__line--wide {
    width: 83.333%;
  }
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}
```

### Notes

- Keep generic spinner and skeleton patterns independent from buttons.
- Button loading is still a button concern, so express it as `.button--loading` with `.button__spinner`.

---

## 7) Turbo-Related Containers

### Blocks

- `.frame-placeholder`
- `.notification-stack`
- `.cart-panel`

### `frame-placeholder` Elements

- `.frame-placeholder__spinner`

### `cart-panel` Elements

- `.cart-panel__body`

### Recommended HTML / ERB

```erb
<turbo-frame id="comments"
             src="<%= comments_path %>"
             class="frame-placeholder"
             loading="lazy">
  <div class="frame-placeholder__spinner">
    <div class="spinner spinner--medium spinner--primary"></div>
  </div>
</turbo-frame>

<div id="shopping-cart" data-turbo-permanent class="cart-panel">
  <div class="cart-panel__body">
    <%# Cart contents preserved during navigation %>
  </div>
</div>
```

### Recommended SCSS

```scss
.frame-placeholder {
  &__spinner {
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 2rem;
  }
}

.cart-panel {
  position: fixed;
  top: 1rem;
  right: 1rem;
  border-radius: 0.5rem;
  background: #fff;
  box-shadow: 0 8px 24px rgba(17, 24, 39, 0.16);

  &__body {
    padding: 1rem;
  }
}
```

### Notes

- Name persistent UI shells by meaning, not by Turbo itself.
- `frame-placeholder` is useful only when the loading placeholder has its own reusable styling contract.

---

## 8) Real-World Example: Restaurant Card

### Recommended Block

- `.restaurant-card`

### Elements

- `.restaurant-card__header`
- `.restaurant-card__title`
- `.restaurant-card__rating`
- `.restaurant-card__rating-value`
- `.restaurant-card__description`

### Modifiers

- `.restaurant-card--featured`

### Example HTML / ERB

```erb
<div class="restaurant-card">
  <%= link_to restaurant_path(@restaurant), class: "restaurant-card__link" do %>
    <div class="restaurant-card__header">
      <h3 class="restaurant-card__title"><%= @restaurant.name %></h3>

      <div class="restaurant-card__rating">
        <span class="restaurant-card__rating-value"><%= rating_stars %></span>
        <span><%= number_with_precision(@restaurant.rating, precision: 1) %></span>
      </div>
    </div>

    <% if @restaurant.description.present? %>
      <p class="restaurant-card__description">
        <%= truncate(@restaurant.description, length: 150) %>
      </p>
    <% end %>
  <% end %>
</div>
```

### Notes

- When a component has domain-specific meaning and a stable internal structure, give it its own block instead of forcing it into a generic `.card`.
- This is the right place to specialize beyond the generic card primitive.

---

## 9) ViewComponent / Rails Extraction Guidance

### Preferred Approach

- Keep SCSS class decisions inside the component, not at the call site.
- Expose semantic options like `variant: :primary`, `size: :small`, or `interactive: true`.
- Map those options to BEM modifiers in the component Ruby class.

### Example Ruby

```ruby
class ButtonComponent < ViewComponent::Base
  def initialize(text:, url:, variant: :primary, size: :medium, loading: false)
    @text = text
    @url = url
    @variant = variant
    @size = size
    @loading = loading
  end

  def classes
    tokens = ["button", "button--#{@variant}"]
    tokens << "button--#{@size}" unless @size == :medium
    tokens << "button--loading" if @loading
    tokens.join(" ")
  end
end
```

### Example ERB

```erb
<%= link_to @url, class: classes do %>
  <span class="button__text"><%= @text %></span>
  <svg class="button__spinner" aria-hidden="true" viewBox="0 0 24 24"></svg>
<% end %>
```

---

## 10) Suggested SCSS File Structure

```text
app/assets/stylesheets/
  abstracts/
    _tokens.scss
    _mixins.scss
  components/
    _button.scss
    _field.scss
    _checkbox.scss
    _card.scss
    _alert.scss
    _badge.scss
    _spinner.scss
    _skeleton.scss
    _notification-stack.scss
    _cart-panel.scss
    _frame-placeholder.scss
    _restaurant-card.scss
  application.scss
```

### Example `application.scss`

```scss
@use "abstracts/tokens";
@use "abstracts/mixins";

@use "components/button";
@use "components/field";
@use "components/checkbox";
@use "components/card";
@use "components/alert";
@use "components/badge";
@use "components/spinner";
@use "components/skeleton";
@use "components/notification-stack";
@use "components/cart-panel";
@use "components/frame-placeholder";
@use "components/restaurant-card";
```

---

## 11) Naming Summary Table

| Tailwind Pattern | Recommended Block | Common Elements | Common Modifiers |
|---|---|---|---|
| Primary / secondary / danger button | `button` | `__text`, `__icon`, `__spinner` | `--primary`, `--secondary`, `--danger`, `--loading`, `--icon` |
| Text input / select / textarea wrapper | `field` | `__label`, `__control`, `__input`, `__select`, `__textarea`, `__hint`, `__error` | `--invalid`, `--disabled`, `--select`, `--textarea` |
| Checkbox row | `checkbox` | `__input`, `__label`, `__text` | `--disabled`, `--invalid` |
| Basic / hoverable card | `card` | `__header`, `__title`, `__body`, `__footer`, `__actions`, `__description` | `--interactive`, `--hoverable`, `--compact` |
| Success / error / info alert | `alert` | `__inner`, `__icon`, `__content`, `__title`, `__message`, `__dismiss` | `--success`, `--error`, `--info`, `--dismissible` |
| Status badge | `badge` | — | `--active`, `--inactive`, `--deleted`, `--pending` |
| Spinner | `spinner` | — | `--small`, `--medium`, `--large`, `--primary` |
| Skeleton loader | `skeleton` | `__line` | `--text`, `--card` |
| Turbo notification area | `notification-stack` | `__item` | — |
| Permanent cart shell | `cart-panel` | `__body` | — |
| Turbo frame placeholder | `frame-placeholder` | `__spinner` | — |
| Domain-specific restaurant card | `restaurant-card` | `__header`, `__title`, `__rating`, `__description` | `--featured` |

---

## 12) Preferred Defaults for Agentic AI Skills

When generating new markup or SCSS from this reference, default to these decisions:

1. Prefer one semantic block per reusable component.
2. Prefer modifiers over one-off variant classes.
3. Prefer elements only when the part has meaning inside the block.
4. Prefer domain blocks like `restaurant-card` when the structure is stable and specific.
5. Keep JS behavior in `data-controller`, `data-action`, and `data-*` APIs; do not encode Stimulus concerns into class names.
6. Keep layout wrappers separate from component blocks unless layout is a built-in part of the component.
