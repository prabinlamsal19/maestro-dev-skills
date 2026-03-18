---
name: selectors
description: Element targeting with id, text, index, css, relational selectors, traits, and dimension matchers
metadata:
  tags: selectors, id, text, index, css, traits, dimensions, relational
---

## Core Selectors

### By Text (Default)

All text selectors are treated as **regular expressions** by default.

```yaml
- tapOn: "Login"
- tapOn:
    text: ".*Continue.*"   # Regex for partial match
- assertVisible: "Welcome to Dashboard"
```

### By ID (Recommended)

```yaml
- tapOn:
    id: "login_button"

- assertVisible:
    id: "dashboard_header"
```

### By Index

When multiple elements match:

```yaml
- tapOn:
    text: "Item"
    index: 0 # First match (0-indexed)
```

### By CSS (Web Only)

Target web elements using standard CSS selector syntax. Does **not** support regex.

```yaml
- tapOn:
    css: ".secondaryButton"

- assertVisible:
    css: "#main-header"
```

## Combined Selectors

Combine multiple matchers (AND logic):

```yaml
- tapOn:
    id: "button"
    text: "Submit"
    enabled: true
```

## Selector Properties

| Property | Description                             | Example               |
| -------- | --------------------------------------- | --------------------- |
| `id`     | Accessibility ID / Semantics identifier | `id: "login_btn"`     |
| `text`   | Text match (regex by default)           | `text: "Login"`       |
| `index`  | Element index (0-based)                 | `index: 2`            |
| `css`    | CSS selector (web only)                 | `css: ".btn-primary"` |

## Relational Selectors

Select elements relative to another element using position or hierarchy.

### Alignment Selectors

```yaml
# Child of element
- tapOn:
    below: "Username"
    id: "input_field"

# Element above another
- tapOn:
    above: "Submit"
    text: "Terms"

# Left/right of element
- tapOn:
    leftOf: "Cancel"
    text: "OK"

- tapOn:
    rightOf:
      id: "input_text"
```

> **Tip:** Positional selectors use screen bounds. Combine with `id`, `text`, or `enabled` for precision: `tapOn: { id: "icon", rightOf: "Settings", enabled: true }`.

### Parent-Child Selectors

```yaml
# containsChild — match parent with a direct child
- tapOn:
    containsChild:
      text: "Order 12345"

# childOf — match element that is child of a parent
- tapOn:
    text: "Delete"
    childOf:
      id: "basket_container"

# containsDescendants — match view with descendants at any depth
- assertVisible:
    id: "list_item"
    containsDescendants:
      - text: "Wireless Headphones"
      - text: "$99.99"
```

| Selector              | Description                                         |
| --------------------- | --------------------------------------------------- |
| `above`               | View located vertically above anchor                |
| `below`               | View located vertically below anchor                |
| `leftOf`              | View to the left of anchor                          |
| `rightOf`             | View to the right of anchor                         |
| `containsChild`       | Parent that has a direct child matching criteria    |
| `childOf`             | Element that is a child of a specified parent       |
| `containsDescendants` | View containing specific descendants at any depth   |

## State Selectors

All accept a boolean value (`true` or `false`):

| Property   | Description                                  | Example            |
| ---------- | -------------------------------------------- | ------------------ |
| `enabled`  | Element interactive or "grayed out"          | `enabled: true`    |
| `selected` | Currently selected (tabs, segmented controls)| `selected: true`   |
| `checked`  | Toggle state (checkboxes, switches)          | `checked: true`    |
| `focused`  | Currently has keyboard input focus           | `focused: true`    |

```yaml
# Assert button is disabled initially
- assertVisible:
    id: "login_button"
    enabled: false

# Tap once it becomes active
- tapOn:
    id: "login_button"
    enabled: true

# Check toggle state
- assertVisible:
    id: "remember_me_checkbox"
    checked: true
```

## Element Traits

Filter elements by physical characteristics:

| Trait             | Description                                         |
| ----------------- | --------------------------------------------------- |
| `traits: text`      | Any element containing text or accessibility label  |
| `traits: long-text` | Element with 200+ characters                        |
| `traits: square`    | Element where width ≈ height (within 3%)            |

```yaml
# Tap first element with any text
- tapOn:
    traits: text

# Assert long content is visible
- assertVisible:
    traits: long-text

# Tap a square icon next to "Home"
- tapOn:
    traits: square
    rightOf: "Home"
```

## Dimension Matchers

Match elements by pixel dimensions:

| Property    | Description                                    |
| ----------- | ---------------------------------------------- |
| `width`     | Target element width in pixels                 |
| `height`    | Target element height in pixels                |
| `tolerance` | Allow +/- pixels for dimension matching        |

```yaml
# Button exactly 200px wide
- tapOn:
    id: "action_button"
    width: 200

# Icon 48px square with 2px tolerance
- assertVisible:
    id: "settings_icon"
    width: 48
    height: 48
    tolerance: 2
```

> **Tip:** Be careful with hardcoded pixel values across devices. Use `tolerance` for cross-device compatibility.

## Optional Selector

Don't fail if not found:

```yaml
- tapOn:
    optional: true
    text: "Skip"
```

## Point Selector

Tap exact coordinates:

```yaml
- tapOn:
    point: "50%,50%"  # Center of screen (relative)

- tapOn:
    point: "100, 250" # Absolute pixels
```

## Platform-Specific IDs

| Platform       | ID Source                                  |
| -------------- | ------------------------------------------ |
| Flutter        | `Semantics.identifier` or `semanticsLabel` |
| React Native   | `testID` or `accessibilityLabel`           |
| iOS Native     | `accessibilityIdentifier`                  |
| Android Native | `resource-id` or `content-desc`            |
| Web            | `data-testid`, `id` attribute, or `css`    |
