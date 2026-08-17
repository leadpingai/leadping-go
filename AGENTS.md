# AGENTS.md

This file is the operating guide for coding agents working in the public Leadping Go SDK repository. Follow it together with `CONTRIBUTING.md`, `SECURITY.md`, and normal Go conventions.

## Repository purpose

This repository contains the official Go client for the Leadping API. Microsoft Kiota generates the client from Leadping’s OpenAPI contract. Calling applications own authentication, credential storage, retry policy, transport configuration, and logging.

Authoritative public resources:

- API contract: <https://leadping.ai/docs/openapi.json>
- API documentation: <https://leadping.ai/docs/api-reference>
- Authentication discovery: <https://leadping.ai/auth.md>
- Security reporting: `SECURITY.md`

## Understand the change before editing

Classify the request first:

1. Endpoint paths, schemas, required fields, and response behavior belong in the upstream API/OpenAPI contract.
2. Request builders, models, serializers, parsers, and `leadping_open_api_client.go` are generated; regenerate them from a corrected contract instead of masking contract errors locally.
3. Documentation, examples, module metadata, workflows, and contributor files are repository-owned and may be edited directly.
4. If valid OpenAPI produces invalid Go, identify the Kiota generator defect and keep any temporary workaround narrow and documented.

Avoid unrelated regeneration, mass formatting, or dependency churn. Review generated diffs for removed fields, renamed symbols, optionality changes, and changed error mappings.

## Go conventions

- Run `gofmt` on every changed Go file.
- Propagate `context.Context`; do not replace it with global state or background contexts in library code.
- Preserve Kiota interfaces, pointer optionality, parse-node behavior, and serialization contracts.
- Return errors to callers with useful context without exposing credentials or response bodies containing sensitive data.
- Do not introduce a parallel HTTP or JSON abstraction when Kiota already owns that layer.
- Treat exported identifiers and module paths as compatibility-sensitive.

## Authentication and examples

Send Leadping credentials as `Authorization: Bearer <credential>`. Never commit or log real user tokens, WorkOS agent assertions or refresh tokens, organization API keys, or source keys. Use nonfunctional values and environment-based injection in examples. Do not imply the SDK stores or refreshes credentials.

## Validation

For Go or module changes, run:

```bash
gofmt -w <changed-go-files>
go test ./...
go vet ./...
```

Do not run `gofmt` across untouched generated files solely to create churn. Documentation-only changes normally need link, spelling, and example review rather than a full build.

Before handing off, inspect `git diff`, explain any OpenAPI or Kiota change, update usage documentation when behavior changes, and report checks run plus anything not validated.

## Releases and security

Do not create tags, alter module versions or release workflows, or publish artifacts unless explicitly authorized. Report vulnerabilities privately through `SECURITY.md`.
