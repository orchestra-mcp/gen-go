# Usage

## Overview

This module contains the generated Go code from the Orchestra Protobuf schema. It provides all message types used in the Orchestra plugin wire protocol.

**Do not edit files in this module by hand.** They are generated from `proto/orchestra/plugin/v1/plugin.proto`.

## Import

```go
import pluginv1 "github.com/orchestra-mcp/gen-go/orchestra/plugin/v1"
```

## Module Dependency

```
go get github.com/orchestra-mcp/gen-go
```

## Message Types

### Envelope

```go
// Build a request
req := &pluginv1.PluginRequest{
    RequestId: "some-uuid",
    Request: &pluginv1.PluginRequest_ToolCall{
        ToolCall: &pluginv1.ToolRequest{
            ToolName:  "create_feature",
            Arguments: args, // *structpb.Struct
        },
    },
}

// Read a response
resp := &pluginv1.PluginResponse{}
// ... unmarshal ...
switch r := resp.Response.(type) {
case *pluginv1.PluginResponse_ToolCall:
    fmt.Println(r.ToolCall.Success)
case *pluginv1.PluginResponse_StorageRead:
    fmt.Println(string(r.StorageRead.Content))
}
```

### Lifecycle

| Type | Purpose |
|---|---|
| `PluginManifest` | Plugin capabilities declaration (ID, version, provides, needs) |
| `RegistrationResult` | Accepted/rejected with optional reason |
| `BootRequest` / `BootResult` | Initialize plugin with config map |
| `ShutdownRequest` / `ShutdownResult` | Graceful shutdown |
| `HealthRequest` / `HealthResult` | Liveness check with status enum |

### Tools

| Type | Purpose |
|---|---|
| `ToolRequest` | Invoke a tool by name with `structpb.Struct` arguments |
| `ToolResponse` | Success/error result with `structpb.Struct` data |
| `ListToolsRequest` / `ListToolsResponse` | Enumerate available tools |
| `ToolDefinition` | Tool name, description, and JSON Schema |

### Storage

| Type | Purpose |
|---|---|
| `StorageReadRequest` / `StorageReadResponse` | Read file content + metadata + version |
| `StorageWriteRequest` / `StorageWriteResponse` | Write with optimistic concurrency (CAS) |
| `StorageDeleteRequest` / `StorageDeleteResponse` | Delete a file |
| `StorageListRequest` / `StorageListResponse` | List files by prefix and glob pattern |
| `StorageEntry` | File path, size, version, modification time |

### Working with Struct

Tool arguments and results use `google.protobuf.Struct`. Build them with `structpb.NewStruct`:

```go
import "google.golang.org/protobuf/types/known/structpb"

args, err := structpb.NewStruct(map[string]any{
    "project_id": "my-project",
    "title":      "Add login page",
    "priority":   "P1",
})
```

Read values back:

```go
title := args.Fields["title"].GetStringValue()
```

Or use the helpers from `github.com/orchestra-mcp/sdk-go/helpers`:

```go
import "github.com/orchestra-mcp/sdk-go/helpers"

title := helpers.GetString(args, "title")
priority := helpers.GetStringOr(args, "priority", "P2")
```

## Regenerating

When the `.proto` file changes, regenerate with buf:

```bash
cd proto/
buf generate
```

The output lands in `gen-go/orchestra/plugin/v1/plugin.pb.go`.
