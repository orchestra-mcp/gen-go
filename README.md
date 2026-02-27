# Orchestra Gen Go

Generated Go code from the Orchestra plugin Protobuf schema.

## Install

```bash
go get github.com/orchestra-mcp/gen-go
```

## Usage

```go
import pluginv1 "github.com/orchestra-mcp/gen-go/orchestra/plugin/v1"

req := &pluginv1.PluginRequest{
    RequestId: "01234567-89ab-cdef-0123-456789abcdef",
    Request: &pluginv1.PluginRequest_ToolCall{
        ToolCall: &pluginv1.ToolRequest{
            ToolName: "create_feature",
        },
    },
}
```

## Regenerating

Do not edit the generated files manually. Regenerate from the monorepo root:

```bash
make proto
```

This runs `buf generate` against the definitions in [proto/](https://github.com/orchestra-mcp/proto).

## File Structure

```
gen-go/
└── orchestra/plugin/v1/
    └── plugin.pb.go          # Generated Protobuf types
```

## Related Packages

| Package | Description |
|---------|-------------|
| [proto](https://github.com/orchestra-mcp/proto) | Source `.proto` definitions |
| [sdk-go](https://github.com/orchestra-mcp/sdk-go) | Go plugin SDK |
| [orchestrator](https://github.com/orchestra-mcp/orchestrator) | Central hub |

## License

[MIT](LICENSE)
