# Advanced MVU Forms — Parameterization, Monads, and State Machines

When building complex forms (10+ fields) in Lustre, standard MVU boilerplate explodes. To keep the `update` function maintainable and prevent impossible states, employ the following advanced MVU patterns.

## 1. Parameterized Messages (Typed Discriminants)

Instead of creating one `Msg` variant per field, group events by their domain and use typed discriminants.

**BAD (Msg Explosion):**
```gleam
pub type Msg {
  UserTypedCep(String)
  UserTypedStreet(String)
  UserTypedCity(String)
  UserBlurredCep
  // ... 20 more variants
}
```

**GOOD (Parameterized):**
```gleam
pub type AddressFormField {
  AddressFormFieldCep
  AddressFormFieldStreet
  AddressFormFieldCity
}

pub type Msg {
  UserTypedAddressField(field: AddressFormField, value: String)
  UserBlurredAddressField(field: AddressFormField)
}
```

*Benefit:* The `update` function collapses from 20 branches into 2. A single helper `set_address_field(form, field, value) -> Form` handles all typing logic exhaustively.

---

## 2. Monadic Request Building (`use` syntax)

Building API requests from validated fields requires unpacking `Option` or `Result` types. Do not use giant tuples in `case` statements (Pyramids of Doom). Use Gleam's `use` syntax with `result.try` or `option.then`.

**BAD:**
```gleam
case validate_email(f.email), validate_cpf(f.cpf), validate_name(f.name) {
  Ok(e), Ok(c), Ok(n) -> Ok(Request(e, c, n))
  _, _, _ -> Error(Nil) // Fails to scale past 3 fields
}
```

**GOOD:**
```gleam
import gleam/option
import gleam/result

fn build_request(form: Form) -> Result(UpdateAddressRequest, Nil) {
  // Required fields: short-circuits to Error if any value is missing
  use parsed_street <- result.try(field_state.value(form.street) |> option.to_result(Nil))
  use parsed_cep <- result.try(field_state.value(form.cep) |> option.to_result(Nil))
  
  // Optional fields: extract safely without short-circuiting
  let parsed_number = option.flatten(field_state.value(form.number))

  Ok(customer.UpdateAddressRequest(
    street: parsed_street,
    cep: parsed_cep,
    number: parsed_number,
  ))
}
```

---

## 3. Form-Level State Machines

A form is a state machine. It is either `Idle` (editable), `Submitting` (locked), or `Failed` (editable, showing server error). 

**BAD (Detached Booleans):**
```gleam
pub type CustomerForm {
  CustomerForm(
    name: String,
    saving: Bool, // Detached! User can type while Saving.
  )
}
```

**GOOD (Wrapped FormState):**
Wrap the form data in a `FormState` machine (typically found in `shared/lib/form_state`).

```gleam
pub type FormState(data) {
  Idle(data: data)
  Submitting(data: data)
  Failed(data: data, error: ErrorCode)
}

pub type CustomerDialog {
  CustomerDialog(
    form_state: FormState(CustomerFormData), // Wraps the data
  )
}
```

*Benefit:* 
1. **Updates:** In `update.gleam`, `form_state.map` ignores mutations if the state is `Submitting`. Impossible to mutate data during flight.
2. **Views:** In `view.gleam`, you check `form_state.is_submitting(state)`. If `True`, you pass `disabled: True` down to all form inputs automatically.

---

## 4. Avoiding Boolean Blindness in Forms

When form logic splits paths (e.g. "billing address same as delivery"), do not use `Bool`. Use explicit Sum Types.

**BAD:**
```gleam
billing: AddressForm,
billing_same_as_delivery: Bool,
```

**GOOD:**
```gleam
pub type BillingAddressMode {
  BillingAddressModeSameAsDelivery
  BillingAddressModeSeparate(form: AddressForm)
}
```

*Benefit:* Eliminates "zombie state" (where a boolean is true, but a stale hidden address form still lingers in the background, validating errors invisibly).