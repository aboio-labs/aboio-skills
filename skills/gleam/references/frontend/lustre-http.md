# HTTP Requests with rsvp

rsvp separates *making* HTTP requests from *handling* their responses. You define a `Handler(msg)` that describes how to process the response, then combine it with a request function to get an `effect.Effect(msg)`.

## Handler Constructors

From most to least opinionated:

```gleam
import rsvp

// Decode JSON from 2xx response. Returns JsonError on decode failure,
// HttpError on 4xx/5xx, UnhandledResponse on wrong content-type.
rsvp.expect_json(my_decoder, fn(result) {
  case result {
    Ok(data) -> GotData(data)
    Error(_err) -> GotError("Failed to load data")
  }
})

// Handle plain text from 2xx responses. Validates content-type starts with "text/".
rsvp.expect_text(fn(result) { ... })

// Handle any 2xx response (full Response(String) access).
rsvp.expect_ok_response(fn(result) { ... })

// Handle ANY response regardless of status code. No filtering.
rsvp.expect_any_response(fn(result) { ... })
```

## Convenience Request Functions

```gleam
// GET — returns Effect(msg)
rsvp.get("/api/products", rsvp.expect_json(product_list_decoder, GotProducts))

// POST with JSON body (sets content-type: application/json automatically)
rsvp.post("/api/products", product_json, rsvp.expect_json(product_decoder, GotCreated))

// Also available: rsvp.put, rsvp.patch, rsvp.delete (all take url, json body, handler)
```

## Custom Requests with Headers

For authenticated requests or custom headers, build a `Request` and use `rsvp.send`:

```gleam
import gleam/http
import gleam/http/request

// rsvp.parse_relative_uri resolves "/api/..." against the page origin in the browser.
// gleam/uri.parse requires absolute URIs and will fail on relative paths.
case rsvp.parse_relative_uri("/api/products") {
  Ok(uri) ->
    case request.from_uri(uri) {
      Ok(req) -> {
        let req = req
          |> request.set_method(http.Get)
          |> request.set_header("authorization", "Bearer " <> token)
        rsvp.send(req, rsvp.expect_json(decoder, GotProducts))
      }
      Error(_) -> effect.none()
    }
  Error(_) -> effect.none()
}
```

## Error Type

```gleam
pub type Error {
  BadBody                                     // Invalid/malformed response body
  BadUrl(String)                              // Malformed URL string
  HttpError(response.Response(String))        // Non-2xx status code
  JsonError(json.DecodeError)                 // JSON decoding failure
  NetworkError                                // Connectivity issues
  UnhandledResponse(response.Response(String)) // Handler cannot process response
}
```

## Common Pattern: API Module

Wrap rsvp in a dedicated `api.gleam` module that centralizes auth headers and base URL:

```gleam
// api.gleam
import rsvp

pub fn get(
  token: String,
  path: String,
  handler: rsvp.Handler(msg),
) -> effect.Effect(msg) {
  case rsvp.parse_relative_uri(path) {
    Ok(uri) ->
      case request.from_uri(uri) {
        Ok(req) ->
          rsvp.send(
            req
              |> request.set_method(http.Get)
              |> request.set_header("authorization", "Bearer " <> token),
            handler,
          )
        Error(_) -> effect.none()
      }
    Error(_) -> effect.none()
  }
}

// In update:
fn update(model, msg) {
  case msg {
    UserOpenedProducts ->
      #(Model(..model, products: Loading),
        api.get(model.token, "/api/products",
          rsvp.expect_json(products_decoder, ApiReturnedProducts)))
  }
}
```
