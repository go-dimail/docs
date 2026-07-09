# Generated from OpenAPI

go-dimail is derived from source: the API's OpenAPI 3.1 document is committed at
the repository root as `openapi.json`, and a small in-tree generator turns it
into the typed models and request methods. This keeps the client faithful to the
spec and makes it trivially regenerable.

```sh
go generate ./...
```

runs the generator (`go run ./internal/gen -spec openapi.json -out .`), which
emits three files:

| File | Contents |
| --- | --- |
| `models_gen.go` | 96 types — structs, string enums, scalar aliases |
| `client_gen.go` | 91 methods, one per OpenAPI operation |
| `client_gen_smoke_test.go` | a generated test exercising every method |

The hand-written runtime in `client.go` is **not** generated: it holds the
`Client`, the Basic→bearer authentication, the HTTP transport, the generic
response decoders every method funnels through, and the typed `APIError`.

## How the mapping works

- `anyOf[T, null]` → a pointer field (`*T`), distinguishing "absent" from zero.
- String enums → a named `type X string` with a `const` block.
- Scalar wrapper schemas (e.g. `DomainName`) → type aliases (`type DomainName = string`).
- Path parameters → positional `string` arguments; query parameters → pointer
  arguments; request bodies → a pointer to the generated request struct.
- Responses → `*T` (object), `[]T` (array), a scalar, or just `error` (no content).

## Drift protection

CI runs `go generate ./...` and fails if the checked-in generated files differ
from what the committed spec produces — so the generated code can never silently
fall out of sync with `openapi.json`.
