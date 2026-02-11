# Logging

## Development Debugging: `echo`

Since Gleam 1.1, use the native `echo` keyword for quick debugging instead of `io.println`.

```gleam
/// BAD - Don't import gleam/io for debugging
import gleam/io

fn something() {
  io.println("This is doing something")
}

/// GOOD - Use echo for development debugging
fn something() {
  echo "This is doing something"
  echo user  // Prints any value with its type
}
```

**Note:** `echo` is for development only. Remove before committing or use proper logging for production.

## Production Logging: Wisp

For server-side production logging, use Wisp's logging functions which integrate with Erlang's logger:

```gleam
import wisp

// Log levels (from most to least severe)
wisp.log_error("Database connection failed")     // ErrorLevel
wisp.log_warning("Rate limit approaching")       // WarningLevel
wisp.log_notice("User logged in")                // NoticeLevel
wisp.log_info("Processing order #123")           // InfoLevel

// Configure logger at app startup (sets minimum level to info)
wisp.configure_logger()

// Or set a specific log level
wisp.set_logger_level(wisp.DebugLevel)
```

**Available log levels:**

| Level     | Function      | Use for                   |
| --------- | ------------- | ------------------------- |
| Emergency | -             | System unusable           |
| Alert     | -             | Immediate action required |
| Critical  | -             | Critical conditions       |
| Error     | `log_error`   | Error conditions          |
| Warning   | `log_warning` | Warning conditions        |
| Notice    | `log_notice`  | Normal but significant    |
| Info      | `log_info`    | Informational messages    |
| Debug     | -             | Debug-level messages      |

**Request logging middleware:**

```gleam
// In your router, wrap handlers with log_request
pub fn handle_request(req: Request) -> Response {
  use req <- wisp.log_request(req)  // Logs request/response details
  // ... your routing logic
}
```

## Logging Best Practices

```gleam
/// BAD - Using echo in production code
pub fn create_order(order: Order) -> Result(Order, Error) {
  echo "Creating order"  // Will clutter logs, no log level
  // ...
}

/// BAD - Logging sensitive data
pub fn login(email: String, password: String) -> Result(Session, Error) {
  wisp.log_info("Login attempt: " <> email <> " / " <> password)  // NEVER!
  // ...
}

/// GOOD - Appropriate log levels and safe data
pub fn create_order(order: Order) -> Result(Order, Error) {
  wisp.log_info("Creating order #" <> order.number)
  case do_create(order) {
    Ok(created) -> {
      wisp.log_info("Order #" <> order.number <> " created successfully")
      Ok(created)
    }
    Error(err) -> {
      wisp.log_error("Failed to create order #" <> order.number <> ": " <> error_to_string(err))
      Error(err)
    }
  }
}

/// GOOD - Structured context in logs
pub fn process_webhook(event: WebhookEvent) -> Result(Nil, Error) {
  wisp.log_info("Processing webhook: type=" <> event.type_ <> " id=" <> event.id)
  // ...
}
```

**Guidelines:**

- Use `echo` only during development, remove before committing
- Use `wisp.log_info` for normal operations
- Use `wisp.log_warning` for recoverable issues
- Use `wisp.log_error` for failures that need attention
- Never log passwords, tokens, or PII
- Include context (IDs, types) but not sensitive values
