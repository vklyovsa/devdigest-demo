# API contract gate

Treat every HTTP route, exported function signature and response shape in the
diff as a published contract. A change that an existing caller cannot survive is
a breaking change, and a breaking change without a migration path is CRITICAL.

Flag when the diff:

- **Renames or removes** a route, a query/path parameter, a request field, or a
  response field that callers read.
- **Tightens validation**: a previously optional field made required, a widened
  enum narrowed, a default removed, a new minimum length or range.
- **Changes a type on the wire**: string → number, scalar → object, a single
  value → an array, `null` no longer permitted.
- **Changes status codes or error shapes**: a 200-with-empty-body becoming a
  404, an error envelope losing `code`.
- **Reorders or repurposes positional parameters** of an exported function.

For each, state: which caller breaks, what it sends today, and what it would
receive after the change. A breaking change IS acceptable when the diff also
carries a version bump, a deprecation window, or an adapter — say so instead of
flagging it.

Do not flag additive changes: a new optional field, a new endpoint, a widened
enum, a new nullable column.