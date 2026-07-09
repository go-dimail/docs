# Errors

Every non-2xx response is returned as an `*dimail.APIError`, which carries the
status code, the request that produced it, the raw body, and the FastAPI
`detail` field when present.

```go
_, err := c.GetDomain(ctx, "absent.example")
var apiErr *dimail.APIError
if errors.As(err, &apiErr) {
	fmt.Println(apiErr.StatusCode) // e.g. 404
	fmt.Println(apiErr.Detail)     // parsed "detail" (json.RawMessage)
	if apiErr.NotFound() {
		// handle 404
	}
}
```

## Fields and predicates

| Member | Meaning |
| --- | --- |
| `StatusCode` | the HTTP status code (`int`) |
| `Status` | the HTTP status line |
| `Method`, `URL` | the request that produced the error |
| `Detail` | the parsed FastAPI `detail` (`json.RawMessage`) |
| `Body` | the raw response body (`[]byte`) |
| `NotFound()` | status is 404 |
| `Unauthorized()` | status is 401 |
| `Forbidden()` | status is 403 |
| `Conflict()` | status is 409 |

`Error()` renders as `dimail: <method> <url>: <status> <text>: <detail>`.
