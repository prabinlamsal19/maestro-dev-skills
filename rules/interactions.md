---
name: interactions
description: tapOn, inputText, scroll, swipe, and gesture commands
metadata:
  tags: tap, input, scroll, swipe, gestures, keyboard
---

## Tap Interactions

### tapOn

```yaml
# By text
- tapOn: "Login"

# By ID
- tapOn:
    id: "submit_button"

# With retry
- tapOn:
    text: "Next"
    retryTapIfNoChange: true
```

### doubleTapOn

```yaml
- doubleTapOn: "Image to zoom"
- doubleTapOn:
    id: "zoomable_view"
```

### longPressOn

```yaml
- longPressOn: "Item to delete"
- longPressOn:
    id: "context_menu_trigger"
```

## Text Input

### inputText

```yaml
# Simple text
- inputText: "john@example.com"

# With variable
- inputText: ${USERNAME}

# Focus field first
- tapOn:
    id: "email_field"
- inputText: "test@example.com"
```

### eraseText

```yaml
# Erase specific count
- eraseText:
    charactersToErase: 10

# Erase all (large number)
- eraseText:
    charactersToErase: 100
```

### hideKeyboard

MUST be called before tapping elements obscured by keyboard:

```yaml
- tapOn:
    id: "password_field"
- inputText: "secret123"
- hideKeyboard
- tapOn:
    id: "submit_button"
```

## Scrolling

### scroll

```yaml
# Default scroll down
- scroll

# Scroll in direction
- scroll:
    direction: UP

# Scroll on specific element
- scroll:
    element:
      id: "scrollable_list"
```

### scrollUntilVisible

```yaml
- scrollUntilVisible:
    element: "Footer"

- scrollUntilVisible:
    element:
      id: "load_more_button"
    direction: DOWN
    speed: 40
```

## Swipe Gestures

```yaml
# Swipe left (like dismissing)
- swipe:
    direction: LEFT

# Swipe with start point
- swipe:
    direction: RIGHT
    start: "90%,50%"
    end: "10%,50%"

# Swipe on element
- swipe:
    direction: LEFT
    element:
      id: "swipeable_card"
```

## Common Patterns

### Form Filling

```yaml
- tapOn:
    id: "name_field"
- inputText: "John Doe"

- tapOn:
    id: "email_field"
- inputText: "john@example.com"

- tapOn:
    id: "password_field"
- inputText: "SecurePass123"

- hideKeyboard
- scroll
- tapOn:
    id: "submit_button"
```

### Carousel Navigation

```yaml
- repeat:
    times: 3
    commands:
      - swipe:
          direction: LEFT
      - takeScreenshot: "slide_${i}"
```

## Hardware Keys

### pressKey

Press physical/hardware keys on the device:

```yaml
# Press enter to submit
- pressKey: "enter"

# Press home button
- pressKey: "home"

# Press back (Android only)
- pressKey: "back"
```

Supported keys: `home`, `lock`, `enter`, `backspace`, `volume up`, `volume down`, `back` (Android), `power` (Android), `tab` (Android). Android TV remote keys also supported.

See [commands.md](../commands.md#presskey-supported-keys) for the full key list.
