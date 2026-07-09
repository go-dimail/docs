# Usage

Every API operation is a method on `*dimail.Client`, named after its OpenAPI
`operationId` (e.g. `GetDomain`, `PostMailboxV2`). Each takes a
`context.Context`, then path parameters, optional query parameters (as pointers —
`nil` leaves them unset), and a request body where applicable.

```go
ctx := context.Background()
c := dimail.NewClient(dimail.WithBasicAuth("apiuser", "apipass"))
if _, err := c.Login(ctx); err != nil {
	log.Fatal(err)
}

// List the domains this user administers.
domains, err := c.GetDomains(ctx)
if err != nil {
	log.Fatal(err)
}
for _, d := range domains {
	fmt.Printf("%s (state=%s)\n", d.Name, d.State)
}

// Create a mailbox (v2 API).
mb, err := c.PostMailboxV2(ctx, "example.gouv.fr", "jean.dupont", &dimail.CreateMailbox2{
	Features: []dimail.MailboxFeature{dimail.MailboxFeatureOX},
})
if err != nil {
	log.Fatal(err)
}
fmt.Println("created:", mb.Email)
```

## Return shapes

| Operation kind | Returns |
| --- | --- |
| Fetch one object | `*T` (e.g. `*Domain`) |
| List | `[]T` (e.g. `[]Domain`) |
| Create / update returning an object | `*T` |
| No-content (deletes, some actions) | just `error` |
| Scalar (a bool/string endpoint) | the value |

## Optional query parameters

Query parameters are pointer arguments; pass `nil` to omit them:

```go
tok, err := c.GetToken(ctx, nil)                 // no username filter
list, err := c.GetMailboxesV2(ctx, "d.fr", nil)  // with_extras unset
```
