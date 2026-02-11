# Lustre Effects & Context

Effects are data describing side effects, not executed code.

## Creating Effects

```gleam
// Return effects from init/update
fn init(_) -> #(Model, Effect(Msg)) {
  #(Loading, fetch_data(ApiReturnedData))
}

// No effect needed
#(model, effect.none())

// Multiple effects
#(model, effect.batch([effect1, effect2]))

// Custom effect
fn my_effect(callback: fn(Result) -> Msg) -> Effect(Msg) {
  effect.from(fn(dispatch) {
    let result = do_something()
    dispatch(callback(result))
  })
}
```

## Effect Timing

Execution order: `update` -> `effect.from` -> `view` -> `before_paint` -> browser paint -> `after_paint`

```gleam
// Synchronous: runs after update, BEFORE view renders
// Use for non-DOM side effects (HTTP, storage, timers)
effect.from(fn(dispatch) { ... })

// Runs after view, before paint - element exists but not painted
// Use for DOM measurement, layout calculations
// `root` is the component's shadow root (Dynamic)
effect.before_paint(fn(dispatch, root) { ... })

// Runs after browser paint completes
// Use for animations, focus, scroll positioning
effect.after_paint(fn(dispatch, root) { ... })
```

## Paint-Cycle Effects (Lustre 5.x)

Paint-cycle effects run after the view is rendered to the DOM but before/after the browser paints pixels.

```gleam
// before_paint: DOM exists but not visible yet
// Good for: measuring elements, setting scroll position, layout prep
effect.before_paint(fn(dispatch, root) {
  // `root` is Dynamic — the component's shadow root in Web Components,
  // or the mount element in SPAs
  // Use FFI-backed decoders to work with it
  Nil
})

// after_paint: pixels are on screen
// Good for: focus management, animations, scroll-into-view
effect.after_paint(fn(dispatch, root) {
  focus_element("input-field")
  Nil
})
```

## Context Propagation (Lustre 5.x)

Parent components can provide context values that child components receive. This enables communication without explicit prop drilling.

```gleam
// Parent provides context (in update function, returned as effect)
effect.provide("accordion", json.object([
  #("value", json.string(active_item)),
  #("multiple", json.bool(False)),
]))

// Child listens for context changes (in component options)
lustre.component(init, update, view, [
  component.on_context_change("accordion", accordion_context_decoder),
])
```

Context is hierarchical: if multiple ancestors provide the same key, the nearest parent's value wins.

## Context System (Parent-Child Communication)

The context system enables hierarchical component communication without prop drilling.

### Pattern: Parent Provides, Children Consume

```gleam
// === PARENT COMPONENT (e.g., accordion) ===

fn update(model, msg) {
  case msg {
    ItemToggled(id) -> {
      let new_value = toggle(model.value, id)
      let model = Model(..model, value: new_value)
      // Provide context to all descendant components
      #(model, effect.provide("accordion", encode_context(model)))
    }
  }
}

fn encode_context(model: Model) -> Json {
  json.object([
    #("value", json.string(model.value)),
    #("multiple", json.bool(model.multiple)),
  ])
}

// === CHILD COMPONENT (e.g., accordion-item) ===

pub fn register() -> Result(Nil, lustre.Error) {
  let app = lustre.component(init, update, view, [
    // Listen for context from nearest ancestor providing "accordion"
    component.on_context_change("accordion", context_decoder),
  ])
  lustre.register(app, "accordion-item")
}

fn context_decoder(dyn) {
  let decoder = {
    use value <- decode.field("value", decode.string)
    use multiple <- decode.field("multiple", decode.bool)
    decode.success(ContextChanged(value, multiple))
  }
  decode.run(dyn, decoder)
}

fn update(model, msg) {
  case msg {
    ContextChanged(value, multiple) -> {
      let is_open = value == model.id
      #(Model(..model, is_open: is_open, multiple: multiple), effect.none())
    }
  }
}
```

### Context vs Events vs Attributes

| Mechanism | Direction | Data flow | Use for |
|---|---|---|---|
| **Context** (`effect.provide`) | Parent -> Children | Broadcast to all descendants | Shared state (theme, accordion value, form state) |
| **Events** (`event.emit`) | Child -> Parent | Bubble up to listeners | User interactions (selected, toggled, submitted) |
| **Attributes** (`on_attribute_change`) | Parent -> Child | Direct parent to child | Configuration (value, disabled, variant) |
| **Properties** (`on_property_change`) | Parent -> Child | Direct, any JSON value | Complex data (lists, objects) |

## Effect Libraries

- `rsvp`: HTTP requests
- `modem`: Routing and URL management
- `plinth`: Browser/Node.js APIs
