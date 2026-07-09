# go-dimail

A pure-Go (CGO-free) client for the **Dimail API** — the mail-hosting management
API of the French government's *La Suite numérique* platform
(<https://api.osprod.dimail1.numerique.gouv.fr>).

The typed models and request methods are **generated from the API's own OpenAPI
document** (`openapi.json`, committed at the repository root) by the generator in
[`internal/gen`](https://github.com/go-dimail/dimail/tree/main/internal/gen).
Everything the API exposes — **91 operations** across users, domains, mailboxes
(v1 and v2), aliases, forwards, allows, identities, app-passwords, the `/my`
self-service views, and the `/system` and `/logs` administration surface — is
available with concrete Go types (**96** of them).

## Install

```sh
go get github.com/go-dimail/dimail
```

## Highlights

- **CGO-free** — the same test binary runs identically on all six supported
  64-bit targets (`amd64`, `arm64`, `riscv64`, `loong64`, `ppc64le`, `s390x`).
- **100 % test coverage**, race-clean, enforced in CI.
- **Reproducible** — the spec and the generator both live in the tree; CI fails
  if the checked-in generated code drifts from the spec. Regenerate with
  `go generate ./...`.

## Next steps

- [Authentication](authentication.md) — Basic → bearer token flow.
- [Usage](usage.md) — the client, options, and calling operations.
- [Errors](errors.md) — the typed `APIError`.
- [Generated from OpenAPI](generator.md) — how the models and methods are built.

The Ruby-idiomatic face of this client lives in the sibling org
[go-ruby-dimail](https://github.com/go-ruby-dimail/dimail).
