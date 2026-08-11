[![](https://img.shields.io/github/v/release/leadpingai/leadping-go?style=for-the-badge)](https://github.com/leadpingai/leadping-go/releases)
[![](https://img.shields.io/github/actions/workflow/status/leadpingai/leadping-go/release.yml?style=for-the-badge)](https://github.com/leadpingai/leadping-go/actions/workflows/release.yml)
[![](https://img.shields.io/github/go-mod/go-version/leadpingai/leadping-go?style=for-the-badge)](https://pkg.go.dev/github.com/leadpingai/leadping-go)
[![](https://img.shields.io/github/actions/workflow/status/leadpingai/leadping-go/codeql.yml?label=CodeQL&style=for-the-badge)](https://github.com/leadpingai/leadping-go/actions/workflows/codeql.yml)

# ![Leadping](https://leadping.ai/favicon.ico) Leadping Go SDK

The official, type-safe Go SDK for the Leadping API. Use it to integrate lead management, conversations, SMS and calling, automations, reporting, billing, and business settings into Go services.

The module is generated from the [Leadping OpenAPI specification](https://leadping.ai/docs/openapi.json) with Microsoft Kiota. It contains request builders and models; your application supplies the HTTP request adapter, credentials, retry policy, and credential storage.

## Installation

Install the SDK and a Go-compatible version of Kiota's `net/http` adapter:

```bash
go get github.com/leadpingai/leadping-go
go get github.com/microsoft/kiota-http-go@v1.4.0
```

## Authentication

Set `LEADPING_API_KEY` to a WorkOS organization API key (`sk_...`). The SDK sends it as `Authorization: Bearer <credential>`. User access tokens are also supported when acting for a signed-in user; `lp_src_...` keys are only for lead-ingestion endpoints. See [API authentication](https://leadping.ai/docs/api-authentication).

## Create a client

Kiota's API-key authentication provider can place the complete Bearer value in the `Authorization` header:

```go
package main

import (
    "context"
    "fmt"
    "os"

    leadping "github.com/leadpingai/leadping-go"
    auth "github.com/microsoft/kiota-abstractions-go/authentication"
    kiotahttp "github.com/microsoft/kiota-http-go"
)

func main() {
    credential := os.Getenv("LEADPING_API_KEY")
    if credential == "" {
        panic("LEADPING_API_KEY is not set")
    }

    authProvider, err := auth.NewApiKeyAuthenticationProviderWithValidHosts(
        "Bearer "+credential,
        "Authorization",
        auth.HEADER_KEYLOCATION,
        []string{"api.leadping.ai"},
    )
    if err != nil {
        panic(err)
    }

    adapter, err := kiotahttp.NewNetHttpRequestAdapter(authProvider)
    if err != nil {
        panic(err)
    }

    client := leadping.NewLeadpingOpenApiClient(adapter)

    lead, err := client.Leads().ById("lead-id").Get(context.Background(), nil)
    if err != nil {
        panic(err)
    }

    fmt.Println(lead.GetId())
}
```

The client defaults to `https://api.leadping.ai`.

## Common operations

Request builders mirror the API path. Methods such as `ById` select a resource; terminal methods send the request.

```go
ctx := context.Background()

// Requires a user access token.
currentUser, err := client.Users().Me().Get(ctx, nil)

// Retrieve organization resources by ID.
source, err := client.Sources().ById("source-id").Get(ctx, nil)
lead, err := client.Leads().ById("lead-id").Get(ctx, nil)
```

Create and update operations accept generated request-model interfaces from the `models` package. Pass the caller's `context.Context` to each SDK method.

## Resources

- [Leadping introduction](https://leadping.ai/docs/introduction)
- [API authentication](https://leadping.ai/docs/api-authentication)
- [API reference](https://leadping.ai/docs/api-reference)
- [OpenAPI specification](https://leadping.ai/docs/openapi.json)
- [Go package documentation](https://pkg.go.dev/github.com/leadpingai/leadping-go)
- [License](LICENSE)
