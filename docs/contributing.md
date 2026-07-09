# Contributing

go-dimail is BSD-3-Clause. The code lives at
[github.com/go-dimail/dimail](https://github.com/go-dimail/dimail).

## Ground rules

- **CGO-free.** No cgo — the package builds and tests on all six supported 64-bit
  targets (`amd64`, `arm64`, `riscv64`, `loong64`, `ppc64le`, `s390x`).
- **100 % coverage.** The CI gate enforces 100 % statement coverage, including
  error branches. The generated methods are covered by a generated smoke test;
  the hand-written runtime is covered by `client_test.go`.
- **Generated code is generated.** Do not hand-edit `models_gen.go`,
  `client_gen.go`, or `client_gen_smoke_test.go`. Change `openapi.json` (or the
  generator in `internal/gen`) and run `go generate ./...`.

## Build & test

```sh
go build ./...
go vet ./...
go generate ./...            # must produce no diff
COVERPKG=$(go list ./... | grep -vE '/internal/gen$' | paste -sd, -)
go test -race -coverpkg="$COVERPKG" -coverprofile=cover.out ./...
go tool cover -func=cover.out | tail -1   # must read 100.0%
```

CI additionally runs the suite under QEMU for the four non-native architectures.
