[![](https://img.shields.io/github/v/release/leadpingai/leadping-go?style=for-the-badge)](https://github.com/leadpingai/leadping-go/releases)
[![](https://img.shields.io/github/actions/workflow/status/leadpingai/leadping-go/release.yml?style=for-the-badge)](https://github.com/leadpingai/leadping-go/actions/workflows/release.yml)
[![](https://img.shields.io/github/go-mod/go-version/leadpingai/leadping-go?style=for-the-badge)](https://pkg.go.dev/github.com/leadpingai/leadping-go)
[![](https://img.shields.io/github/actions/workflow/status/leadpingai/leadping-go/codeql.yml?label=CodeQL&style=for-the-badge)](https://github.com/leadpingai/leadping-go/actions/workflows/codeql.yml)

# Leadping Go SDK

Type-safe Go client for the Leadping API.

## Install

```bash
go get github.com/leadpingai/leadping-go
```

The generated client uses a Kiota request adapter. Install the default HTTP adapter:

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
    adapter := createLeadpingRequestAdapter()
    client := leadping.NewLeadpingOpenApiClient(adapter)

    me, err := client.Users().Me().Get(context.Background(), nil)
    if err != nil {
        panic(err)
    }

    _ = me
}
```

`createLeadpingRequestAdapter` is application code. Configure it to send one of:

- `Authorization: Bearer <token>`
- `X-Leadping-Api-Key: <key>`

The client defaults to `https://api.leadping.ai` when the adapter does not already have a base URL.

## Links

- [Documentation](https://leadping.ai/docs)
- [API reference](https://leadping.ai/docs/api-reference)
- [License](LICENSE)
