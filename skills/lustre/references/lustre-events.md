# Events

## Standard Events

```gleam
event.on_click(UserClickedButton)
event.on_input(UserUpdatedField)
event.on_submit(UserSubmittedForm)
event.on("change", change_decoder)
```

## Custom Events (Lustre 5.x)

Child-to-parent communication via custom DOM events:

```gleam
// Child component emits a custom event (in view)
html.button([event.on_click(Clicked)], [html.text("Click")])

// In child's update, emit event to parent:
fn update(model, msg) {
  case msg {
    Clicked -> #(model, event.emit("item-selected", json.string(model.value)))
  }
}

// Parent listens on the custom element
html.element("my-selector", [
  event.on("item-selected", selected_decoder),
], [])
```

## Advanced Events (Lustre 5.x)

`event.advanced` decodes into an `event.handler(...)` record to control `prevent_default` and `stop_propagation` alongside message dispatch. The decoder must return `event.handler(dispatch:, prevent_default:, stop_propagation:)`, NOT a two-tuple.

```gleam
// DON'T — stale two-tuple API (does not compile in current Lustre)
event.advanced("keydown", fn(event) {
  use key <- decode.field("key", decode.string)
  decode.success(#(UserPressedKey(key), event.prevent_default()))
})

// DO — current record API
event.advanced("keydown", {
  use key <- decode.field("key", decode.string)
  decode.success(event.handler(
    dispatch: UserPressedKey(key),
    prevent_default: key == "ArrowDown" || key == "ArrowUp",
    stop_propagation: False,
  ))
})
```

Key rules:
- `prevent_default` and `stop_propagation` are `Bool`, so they can be conditional on the decoded event value.
- Set `stop_propagation: False` unless a competing ancestor handler must be suppressed. Prefer `False` to preserve global shortcut handlers (Cmd+S, Esc) and bubbling accessibility contracts.
- Use `event.on` for simple cases where neither flag is needed. Use `event.advanced` only when you need `preventDefault` or `stopPropagation`.
- `event.prevent_default` (the pipe helper) still works for single-attribute chaining: `event.on("mousedown", decoder) |> event.prevent_default`. Use `event.advanced` when the decision is conditional on the decoded value.

## Debounce and Throttle (Lustre 5.x)

```gleam
// Debounce: wait for pause in events (e.g., search input)
event.debounce(event.on_input(UserTypedSearch), 300)

// Throttle: limit event frequency (e.g., scroll handler)
event.throttle(event.on("scroll", scroll_decoder), 100)
```

**Debounce + controlled inputs caveat:** Never wrap `on_input` with `event.debounce` on a controlled input (one with `attribute.value(model.x)`). The delayed dispatch keeps the model stale, and the VDOM diff resets the DOM input. Use length guards in `update` instead — see Gotchas §13.

## Keyboard Navigation Patterns

### Scoping prevent_default Narrowly

When handling arrow keys in components like comboboxes or menus, only prevent default for the specific keys that need it. Blanket-preventing all keydown defaults breaks Enter (form submit), Escape (close), Tab (focus), and typing.

```gleam
event.advanced("keydown", {
  use key <- decode.field("key", decode.string)
  decode.success(event.handler(
    dispatch: UserPressedKey(key),
    // Only prevent scroll on arrow keys
    prevent_default: key == "ArrowDown" || key == "ArrowUp",
    stop_propagation: False,
  ))
})
```

### Combobox aria-activedescendant

For keyboard-navigable lists (autocomplete, combobox), set `aria-activedescendant` on the input to the ID of the highlighted item. Without this, screen readers cannot announce the keyboard-highlighted option.

```gleam
html.input([
  attribute.role("combobox"),
  attribute.attribute("aria-expanded", case model.open {
    True -> "true"
    False -> "false"
  }),
  attribute.attribute("aria-activedescendant", case model.highlighted_index >= 0 {
    True -> "result-" <> int.to_string(model.highlighted_index)
    False -> ""
  }),
  // ... event handlers
])

// Each result item needs a matching ID
list.index_map(results, fn(item, i) {
  html.button([
    attribute.id("result-" <> int.to_string(i)),
    attribute.role("option"),
    // ...
  ], [html.text(item.label)])
})
```
