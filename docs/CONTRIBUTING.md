# Contributing to gen-go

## Important

This module contains **generated code**. Do not edit `plugin.pb.go` directly. Changes to the wire protocol must be made in the [proto](https://github.com/orchestra-mcp/proto) repository.

## Prerequisites

- Go 1.23+
- [buf CLI](https://buf.build/docs/installation) v1.28+
- `protoc-gen-go` plugin: `go install google.golang.org/protobuf/cmd/protoc-gen-go@latest`

## Regenerating

1. Clone the proto repository and make your changes to `plugin.proto`.
2. Run code generation:
   ```bash
   cd proto/
   buf generate
   ```
3. The generated `.pb.go` file will be placed in `gen-go/orchestra/plugin/v1/`.
4. Commit the regenerated file along with the proto changes.

## Development Setup

```bash
git clone https://github.com/orchestra-mcp/gen-go.git
cd gen-go
go build ./...
```

## Code Style

- `gofmt` formatting is enforced.
- Do not add hand-written code to this module. It exists solely for generated types.

## Testing

Verify the generated code compiles:

```bash
go build ./...
go vet ./...
```

## Pull Request Process

1. Fork the repository and create a feature branch.
2. If changing the proto schema, submit a PR to `proto` first.
3. Regenerate, verify the build passes.
4. Open a PR with both the regenerated code and a reference to the proto change.

## Related Repositories

- [orchestra-mcp/proto](https://github.com/orchestra-mcp/proto) -- Protobuf schema (source of truth)
- [orchestra-mcp/sdk-go](https://github.com/orchestra-mcp/sdk-go) -- Go Plugin SDK
- [orchestra-mcp](https://github.com/orchestra-mcp) -- Organization home
