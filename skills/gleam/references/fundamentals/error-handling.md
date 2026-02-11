# Error Handling

## Error Handling Discipline

**NEVER silently discard errors.** Silent error discards make debugging impossible.

- **NEVER write `Error(_) ->`** — always bind the error and log it:
  ```gleam
  // WRONG
  Error(_) -> default_value

  // RIGHT
  Error(err) -> {
    logging.log(logging.Warning, "context: " <> string.inspect(err))
    default_value
  }
  ```

- **NEVER write `fn(_err) {`** in error handlers — bind and log:
  ```gleam
  // WRONG
  result.map_error(fn(_) { ServerError("parse failed") })

  // RIGHT
  result.map_error(fn(err) { ServerError("parse failed: " <> string.inspect(err)) })
  ```

- **Exception**: `Error(_)` is OK when the error type is `Nil` (e.g., `int.parse`, `dict.get`) since there's nothing to inspect. `Ok(_)` to discard success values is also fine.

## Unified Error Handling

Create a unified error type with HTTP status mapping:

```gleam
pub type AppError {
  BadRequest(String)           // 400
  Unauthorized(String)         // 401
  Forbidden(String)            // 403
  NotFound(String)             // 404
  UnprocessableContent(String) // 422
  InternalServerError(String)  // 500
}
```

## Philosophy: Errors as Values

Gleam uses `Result` types for error handling. There are no exceptions. Every function that can fail returns `Result(SuccessType, ErrorType)`.

## Domain-Specific Errors

For better type safety, create domain-specific error types in your entity modules:

```gleam
// catalog/product.gleam
pub type ProductError {
  NotFound(Uuid)
  ValidationFailed(String)
  DatabaseError(String)
}

// auth/session.gleam
pub type SessionError {
  InvalidCredentials
  EmailAlreadyExists
  Expired
  InvalidToken
}

// order/order.gleam
pub type OrderError {
  NotFound(String)
  ValidationError(String)
  DatabaseError(String)
}
```

These errors are self-documenting and can be pattern-matched precisely.
At the handler boundary, map domain errors to HTTP responses.

## Application Error Type

Create an application-level error type that wraps domain errors and provides cross-cutting concerns:

```gleam
// error/error.gleam

import catalog/product
import auth/session
import order/order

pub type AppError {
  // Domain error wrappers
  ProductErr(product.ProductError)
  SessionErr(session.SessionError)
  OrderErr(order.OrderError)

  // Cross-cutting concerns (for cases not covered by domain errors)
  ValidationErr(String)          // Generic input validation
  AuthenticationErr(String)      // Auth failures
  AuthorizationErr(String)       // Permission failures
  ResourceNotFoundErr(String)    // Generic not found
  ConflictErr(String)            // Resource conflicts
  InternalErr(String)            // Unexpected errors
}
```

**Note:** For your specific project, rename `AppError` to something project-specific like `AcmeError`, `ShopError`, or `MyAppError`. This avoids confusion with generic "Error" types.

## Helper Functions

```gleam
/// Get HTTP status code for an error
pub fn to_status_code(error: AppError) -> Int {
  case error {
    // Domain errors - map to appropriate status
    ProductErr(product.NotFound(_)) -> 404
    ProductErr(product.ValidationFailed(_)) -> 422
    ProductErr(product.DatabaseError(_)) -> 500
    SessionErr(session.InvalidCredentials) -> 401
    SessionErr(session.EmailAlreadyExists) -> 409
    SessionErr(session.Expired) -> 401
    SessionErr(session.InvalidToken) -> 401
    OrderErr(order.NotFound(_)) -> 404
    OrderErr(order.ValidationError(_)) -> 422
    OrderErr(order.DatabaseError(_)) -> 500

    // Cross-cutting errors
    ValidationErr(_) -> 400
    AuthenticationErr(_) -> 401
    AuthorizationErr(_) -> 403
    ResourceNotFoundErr(_) -> 404
    ConflictErr(_) -> 409
    InternalErr(_) -> 500
  }
}

/// Extract message from any error variant
pub fn message(error: AppError) -> String {
  case error {
    ProductErr(product.NotFound(id)) -> "Product not found: " <> uuid.to_string(id)
    ProductErr(product.ValidationFailed(msg)) -> msg
    ProductErr(product.DatabaseError(msg)) -> msg
    SessionErr(session.InvalidCredentials) -> "Invalid credentials"
    SessionErr(session.EmailAlreadyExists) -> "Email already exists"
    SessionErr(session.Expired) -> "Session expired"
    SessionErr(session.InvalidToken) -> "Invalid token"
    OrderErr(order.NotFound(msg)) -> msg
    OrderErr(order.ValidationError(msg)) -> msg
    OrderErr(order.DatabaseError(msg)) -> msg
    ValidationErr(msg) -> msg
    AuthenticationErr(msg) -> msg
    AuthorizationErr(msg) -> msg
    ResourceNotFoundErr(msg) -> msg
    ConflictErr(msg) -> msg
    InternalErr(msg) -> msg
  }
}

/// Get error kind as a string (for logging)
pub fn kind(error: AppError) -> String {
  case error {
    ProductErr(product.NotFound(_)) -> "ProductNotFound"
    ProductErr(product.ValidationFailed(_)) -> "ProductValidation"
    ProductErr(product.DatabaseError(_)) -> "ProductDatabase"
    SessionErr(session.InvalidCredentials) -> "InvalidCredentials"
    SessionErr(session.EmailAlreadyExists) -> "EmailExists"
    SessionErr(session.Expired) -> "SessionExpired"
    SessionErr(session.InvalidToken) -> "InvalidToken"
    OrderErr(order.NotFound(_)) -> "OrderNotFound"
    OrderErr(order.ValidationError(_)) -> "OrderValidation"
    OrderErr(order.DatabaseError(_)) -> "OrderDatabase"
    ValidationErr(_) -> "Validation"
    AuthenticationErr(_) -> "Authentication"
    AuthorizationErr(_) -> "Authorization"
    ResourceNotFoundErr(_) -> "NotFound"
    ConflictErr(_) -> "Conflict"
    InternalErr(_) -> "Internal"
  }
}
```

## Error Response Module

Create a response handler that converts errors to HTTP responses:

```gleam
// error/response.gleam

import error/error.{type AppError}
import error/view as error_view
import wisp

pub fn handle(err: AppError) -> wisp.Response {
  // Log the error (full details for debugging)
  wisp.log_error(error.kind(err) <> ": " <> error.message(err))

  // Return appropriate HTTP response (safe message only)
  let status = error.to_status_code(err)
  let body = error_view.to_json_string(err)

  wisp.response(status)
  |> wisp.set_header("content-type", "application/json")
  |> wisp.string_body(body)
}
```

## Error View Module

Serialize errors to JSON (hiding internal details from clients):

```gleam
// error/view.gleam

import error/error.{type AppError}
import gleam/json

pub fn to_json(err: AppError) -> json.Json {
  json.object([
    #("error", json.string(safe_message(err))),
    #("code", json.int(error.to_status_code(err))),
  ])
}

pub fn to_json_string(err: AppError) -> String {
  to_json(err) |> json.to_string
}

/// Return user-safe message (no internal details)
fn safe_message(err: AppError) -> String {
  case err {
    // Domain errors - provide specific but safe messages
    error.ProductErr(product.NotFound(_)) -> "Product not found"
    error.ProductErr(product.ValidationFailed(msg)) -> msg
    error.ProductErr(product.DatabaseError(_)) -> "Database error"
    error.SessionErr(session.InvalidCredentials) -> "Invalid credentials"
    error.SessionErr(session.EmailAlreadyExists) -> "Email already in use"
    error.SessionErr(session.Expired) -> "Session expired"
    error.SessionErr(session.InvalidToken) -> "Invalid token"
    error.OrderErr(order.NotFound(_)) -> "Order not found"
    error.OrderErr(order.ValidationError(msg)) -> msg
    error.OrderErr(order.DatabaseError(_)) -> "Database error"

    // Cross-cutting errors
    error.ValidationErr(msg) -> msg
    error.AuthenticationErr(_) -> "Authentication required"
    error.AuthorizationErr(_) -> "Access denied"
    error.ResourceNotFoundErr(msg) -> msg
    error.ConflictErr(msg) -> msg
    error.InternalErr(_) -> "An unexpected error occurred"
  }
}
```

## Error Propagation Pattern

Use `result.try` to chain operations that might fail:

```gleam
import gleam/result

pub fn process_request(req: Request, db: Connection) -> Response {
  let result = {
    // Parse and validate input
    use user_id <- result.try(
      uuid.from_string(req.user_id)
      |> result.replace_error(error.ValidationErr("Invalid user ID"))
    )

    use tenant_id <- result.try(
      uuid.from_string(req.tenant_id)
      |> result.replace_error(error.ValidationErr("Invalid tenant ID"))
    )

    // Call entity layer - returns domain error
    use data <- result.try(
      product.get(db, user_id, tenant_id)
      |> result.map_error(error.ProductErr)
    )

    // Transform result
    Ok(view.to_json(data))
  }

  case result {
    Ok(json) -> json_response(json, 200)
    Error(err) -> error_response.handle(err)
  }
}
```

## Layer-Specific Error Handling

### Handler Layer

HTTP handlers parse requests, call entity functions, and map errors to HTTP responses.

```gleam
// catalog/handler.gleam
pub fn create(req: Request, db: Connection, tenant_id: String) -> Response {
  use json_body <- wisp.require_json(req)

  let result = {
    use tid <- result.try(
      uuid.from_string(tenant_id)
      |> result.replace_error(error.ValidationErr("Invalid tenant ID"))
    )
    use data <- result.try(
      decode.run(json_body, view.decoder())
      |> result.replace_error(error.ValidationErr("Invalid request body"))
    )
    // Call entity function - map domain error to app error
    product.create(db, tid, data)
    |> result.map_error(error.ProductErr)
  }

  case result {
    Ok(record) -> {
      wisp.response(201)
      |> wisp.set_header("content-type", "application/json")
      |> wisp.string_body(json.to_string(view.to_json(record)))
    }
    Error(err) -> error_response.handle(err)
  }
}
```

### Entity Layer

Entity modules contain types, pure functions, and database operations. They return domain-specific errors:

```gleam
// catalog/product.gleam
pub type ProductError {
  NotFound(Uuid)
  ValidationFailed(String)
  DatabaseError(String)
}

/// Create a product - returns domain error
pub fn create(db: pog.Connection, tenant_id: Uuid, data: CreateRequest)
  -> Result(sql.CreateProductRow, ProductError) {
  // Validation
  case string.is_empty(data.title) {
    True -> Error(ValidationFailed("Title is required"))
    False -> {
      case sql.create_product(db, tenant_id, data.title, data.handle) {
        Ok(returned) -> extract_first(returned)
        Error(_) -> Error(DatabaseError("Database error"))
      }
    }
  }
}

pub fn get_by_id(db: pog.Connection, id: Uuid, tenant_id: Uuid)
  -> Result(sql.GetProductRow, ProductError) {
  case sql.get_product(db, id, tenant_id) {
    Ok(returned) -> extract_first(returned)
    Error(_) -> Error(DatabaseError("Database error"))
  }
}

fn extract_first(returned: pog.Returned(a)) -> Result(a, ProductError) {
  case returned.rows {
    [row] -> Ok(row)
    [] -> Error(NotFound(uuid.v7()))  // or pass the actual ID
    _ -> Error(DatabaseError("Unexpected result"))
  }
}
```

## Converting External Errors

When integrating with external services that have their own error types:

```gleam
// Map external error to app error
fn external_to_error(e: ExternalError) -> error.AppError {
  case e {
    external.NetworkError(msg) -> error.InternalErr("Network error: " <> msg)
    external.NotFound -> error.ResourceNotFoundErr("External resource not found")
    external.RateLimited -> error.InternalErr("Rate limited by external service")
    external.Unauthorized -> error.AuthenticationErr("External service authentication failed")
  }
}

// Usage
pub fn fetch_external_data(client: Client, id: String)
  -> Result(Data, error.AppError) {
  case external_service.get(client, id) {
    Ok(data) -> Ok(data)
    Error(e) -> Error(external_to_error(e))
  }
}
```

## Best Practices

### 1. Be Specific with Error Messages

```gleam
// Bad
Error(error.ValidationErr("Invalid input"))

// Good
Error(error.ValidationErr("Email must be a valid email address"))
```

### 2. Don't Expose Internal Details

```gleam
// Bad - exposes internal structure
Error(error.InternalErr("pog.QueryError: constraint violation"))

// Good - generic message
Error(error.InternalErr("Database error"))
// Log the actual error for debugging
```

### 3. Use Appropriate Error Types

```gleam
// Wrong - using generic error for domain concept
Error(error.InternalErr("User not found"))

// Correct - use domain error
Error(error.ProductErr(product.NotFound(id)))
```

### 4. Early Return on Error

```gleam
// Use result.try for early return
use user <- result.try(get_user(db, user_id))
use permissions <- result.try(get_permissions(db, user.id))
use _ <- result.try(check_permission(permissions, "create"))
// Only reaches here if all succeeded
```

### 5. Log Errors at the Right Level

- **Handler**: Log all errors before returning HTTP response
- **Entity**: Log significant business rule violations
- **SQL/View**: Don't log (let handler handle it)
