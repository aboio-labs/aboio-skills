# Common Lustre Gotchas

## 1. Recursive Update Functions

```gleam
// DON'T: Recursive message dispatching
fn update(model, msg) {
  case msg {
    Save -> {
      let new_model = ...
      update(new_model, Validate)  // Anti-pattern!
    }
  }
}

// DO: Extract shared logic into functions
fn update(model, msg) {
  case msg {
    Save -> {
      let validated = validate(model)
      #(save(validated), effect.none())
    }
  }
}
```

## 2. Performing Side Effects in Update

```gleam
// DON'T: Side effects in update
fn update(model, msg) {
  let _ = http.get(url)  // Side effect!
  #(model, effect.none())
}

// DO: Return effects
fn update(model, msg) {
  #(model, rsvp.get(url, handler))
}
```

## 3. Missing Keys on Dynamic Lists

```gleam
// DON'T: Unkeyed lists
html.ul([], list.map(items, view_item))

// DO: Use keyed elements (keys must be Strings!)
keyed.ul([], list.map(items, fn(item) {
  #(int.to_string(item.id), view_item(item))
}))
```

## 4. Mixing Controlled and Uncontrolled Patterns

```gleam
// DON'T: Set value without handling changes (stuck input)
html.input([
  attribute.value(model.name),
  // Missing on_input handler - input appears frozen!
])

// DON'T: Uncontrolled with dynamic default (ignored after mount)
html.input([
  attribute.default_value(model.name),  // Only used on first render
])

// DO: Fully controlled
html.input([
  attribute.value(model.name),
  event.on_input(UserUpdatedName),
])

// DO: Fully uncontrolled (browser manages state)
html.input([
  attribute.name("name"),
  attribute.default_value(initial_value),
])
```

## 5. Forgetting to Shutdown Server Components

```gleam
// DON'T: Memory leak
fn close_socket(state) {
  Nil  // Component keeps running!
}

// DO: Clean up
fn close_socket(state) {
  lustre.shutdown() |> lustre.send(to: state.runtime)
}
```

## 6. Stateful Component Overuse

```gleam
// DON'T: Component for simple display
let component = lustre.component(...)  // Overkill!

// DO: Use view functions
fn view_user_card(user: User) -> Element(msg) {
  html.div([], [html.text(user.name)])
}
```

## 7. Effect Timing Issues

```gleam
// DON'T: DOM query in effect.from (runs BEFORE view!)
effect.from(fn(dispatch) {
  let el = document.get_element_by_id("new-element")
  // ALWAYS nil - view hasn't rendered yet!
})

// DO: Use before_paint for DOM operations after render
effect.before_paint(fn(dispatch, root) {
  let el = document.get_element_by_id("new-element")
  // Element exists - view has rendered
})

// DO: Use after_paint for focus, scroll, animations
effect.after_paint(fn(dispatch, root) {
  focus_element("input-field")
})
```

## 8. Including Non-Serializable Event Data (Server Components)

```gleam
// DON'T: Assume event properties are available
event.on("click", fn(event) {
  // event.target.id won't work in server components
})

// DO: Explicitly include properties
html.button([
  server_component.include(event.on("click", handler), ["target.id"]),
], [...])
```

## 9. before_paint Receives Dynamic, Not a Typed Element

```gleam
// DON'T: Assume root is typed
effect.before_paint(fn(dispatch, root) {
  root.querySelector(".item")  // Won't compile — root is Dynamic
})

// DO: Use FFI-backed decoder for shadow root operations
effect.before_paint(fn(dispatch, root) {
  case element.from_dynamic(root) {
    Ok(el) -> element.query_selector(el, ".item")
    Error(_) -> Nil
  }
})
```

## 10. attribute.property() Doesn't Work in SSR

```gleam
// DON'T: Use property() unconditionally
html.input([attribute.property("value", json.string(model.value))])

// DO: Check runtime and fall back
html.input([
  case lustre.is_browser() {
    True -> attribute.property("value", json.string(model.value))
    False -> attribute.attribute("value", model.value)
  },
  event.on_input(UserTyped),
])
```

## 11. on_attribute_change Fires on Initial Render

```gleam
// DON'T: Assume attribute change means user action
fn update(model, msg) {
  case msg {
    ValueChanged(v) -> #(Model(..model, value: v), do_expensive_work())
    // Fires on mount too! Wastes resources.
  }
}

// DO: Guard with Prop state
fn update(model, msg) {
  case msg {
    ValueChanged(v) -> {
      case model.value.controlled || model.value.touched {
        True -> #(model, effect.none())  // Already managed
        False -> #(Model(..model, value: Prop(..model.value, value: v)), effect.none())
      }
    }
  }
}
```

## 12. Don't Set Component-Managed ARIA Attributes

```gleam
// DON'T: Manually set ARIA that the component manages
accordion.trigger([
  attribute.attribute("role", "button"),         // Component sets this
  attribute.attribute("aria-expanded", "true"),  // Component manages this
  attribute.attribute("aria-controls", "panel"), // Component sets this
], [html.text("Toggle")])

// DO: Let the component handle its own ARIA
accordion.trigger([], [html.text("Toggle")])
// The component internally sets role, aria-expanded, aria-controls, aria-selected
```
