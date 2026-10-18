# Spalk fork of zitadel-go

This fork adds the client for `zitadel.user.v2.UserService/MoveUser`, an RPC that exists only in Spalk's Zitadel fork ([SpalkLtd/zitadel](https://github.com/SpalkLtd/zitadel), branch `spalk`).

- **Branches:** `spalk-v3.26` (on upstream `v3.26.1`, tag `v3.26.1-spalk.1`) is the one Spalk uses: it is the newest upstream release whose `go.mod` and dependencies stay at `go 1.24.0`, which Spalk's Go modules are on. `spalk` (the default branch, on `v3.29.3`, tag `v3.29.3-spalk.1`) needs `go 1.25.0` and is unused.
- **Tags:** `v<upstream version>-spalk.<n>`, e.g. `v3.26.1-spalk.1`.
- **Module path:** unchanged (`github.com/zitadel/zitadel-go/v3`). Spalk's monorepo uses it through a `replace` in the Live Sync API's and api-inspector's `go.mod`.

## Regenerating

The generated code comes from the protos of the Zitadel release this SDK version was generated from (`v4.11.0` for `v3.26.1`, `v4.17.0` for `v3.29.3`), with only the fork's proto change applied, so the diff stays limited to `MoveUser`:

```bash
git -C <zitadel clone> worktree add --detach /tmp/zitadel-protos v4.11.0
git -C <zitadel clone> diff v4.19.4 spalk -- proto | git -C /tmp/zitadel-protos apply
# tools pinned in the Makefile: buf, protoc-gen-go, protoc-gen-go-grpc
find pkg/client/zitadel -name '*.pb.go' -delete
buf generate /tmp/zitadel-protos/proto --template buf.gen.yaml
rm -rf pkg/client/zitadel/resources pkg/client/zitadel/settings/object
go build ./pkg/...
```

Only `pkg/client/zitadel/user/v2/user_service*.pb.go` should change.
