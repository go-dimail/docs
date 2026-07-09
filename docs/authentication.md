# Authentication

Dimail authenticates with HTTP Basic credentials to mint a **bearer token**, then
expects that token on every other call. A `Client` sends a bearer token when one
is set (via `WithToken`, `SetToken`, or a prior `Login`) and otherwise falls back
to the Basic credentials. `Login` relies on that precedence: it runs before a
token exists, so the token request is authenticated with Basic.

```go
package main

import (
	"context"
	"log"

	"github.com/go-dimail/dimail"
)

func main() {
	ctx := context.Background()

	c := dimail.NewClient(dimail.WithBasicAuth("apiuser", "apipass"))
	if _, err := c.Login(ctx); err != nil { // fetches and stores a bearer token
		log.Fatal(err)
	}

	// Subsequent calls use the stored token.
	dom, err := c.GetDomain(ctx, "example.gouv.fr")
	if err != nil {
		log.Fatal(err)
	}
	_ = dom
}
```

If you already hold a token, skip `Login`:

```go
c := dimail.NewClient(dimail.WithToken(os.Getenv("DIMAIL_TOKEN")))
```

## Options

`NewClient` accepts functional options:

| Option | Purpose |
| --- | --- |
| `WithBaseURL(u)` | Override the API root (defaults to production). |
| `WithHTTPClient(h)` | Supply your own `*http.Client` (timeouts, proxy, TLS). |
| `WithBasicAuth(user, pass)` | Credentials used to obtain a token. |
| `WithToken(token)` | Authenticate every request with a bearer token. |
| `WithUserAgent(ua)` | Override the `User-Agent` header. |
