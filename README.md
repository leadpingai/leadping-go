# Leadping Go SDK

Typed Go client for the Leadping API, generated from `leadpingai/openapi` with Kiota.

## Install

```bash
go get github.com/leadpingai/leadping-go
```

Use a Kiota request adapter, such as:

```bash
go get github.com/microsoft/kiota-http-go
```

## Use

```go
package main

import (
    "context"

    leadping "github.com/leadpingai/leadping-go"
)

func main() {
    adapter := CreateLeadpingRequestAdapter()
    client := leadping.NewLeadpingOpenApiClient(adapter)

    me, err := client.Users().Me().Get(context.Background(), nil)
    if err != nil {
        panic(err)
    }

    _ = me
}
```

`CreateLeadpingRequestAdapter` should return a Kiota request adapter configured with your Leadping authentication and `https://api.leadping.ai` as the base URL.

## Release

Publishing is handled by this repository's `Release Go SDK` workflow. Go module consumers install released tags from `github.com/leadpingai/leadping-go`.

## Notes

- Generated code comes from `leadpingai/openapi`; update the OpenAPI spec instead of hand-editing generated files.
- Module path: `github.com/leadpingai/leadping-go`
- License: see `LICENSE`
