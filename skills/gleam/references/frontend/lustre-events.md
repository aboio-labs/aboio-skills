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

`event.advanced` returns a `Handler(msg)` with control over `prevent_default` and `stop_propagation`:

```gleam
// Prevent form submission default behavior
event.advanced("submit", fn(event) {
  use data <- decode.field("target", form_data_decoder)
  decode.success(#(UserSubmittedForm(data), event.prevent_default()))
})

// Stop event propagation (e.g., click on nested element)
event.advanced("click", fn(event) {
  use id <- decode.field("target", decode.at(["id"], decode.string))
  decode.success(#(ItemClicked(id), event.stop_propagation()))
})
```

## Debounce and Throttle (Lustre 5.x)

```gleam
// Debounce: wait for pause in events (e.g., search input)
event.debounce(event.on_input(UserTypedSearch), 300)

// Throttle: limit event frequency (e.g., scroll handler)
event.throttle(event.on("scroll", scroll_decoder), 100)
```
