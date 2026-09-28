# OpenJEV Support

This fork adds optional [OpenJEV](https://openjev.sh) support alongside the
original TypeSafe integration. OpenJEV is a free community gateway to the same
Jev model. TypeSafe remains the default; anyone with a TypeSafe key sees zero
behaviour change.

## What was added

- `internal/jev/client.go` — `OpenJEVEndpoint` and `OpenJEVModel` constants;
  HTTP 503 added to the retryable status set alongside 429 and 529.
- `internal/creds/creds.go` — `Provider` type; `Resolve()` now returns a
  provider following the selection rule below.
- `cmd/jym/main.go` — `newClient()` selects the endpoint/model based on the
  provider; `usage()` documents `OPENJEV_API_KEY` and `JEV_PROVIDER`.
- `cmd/jym/explain.go` — `--explain` shows the correct model for the provider.
- `cmd/jym/setup.go` — setup validates against TypeSafe (unchanged).
- `cmd/jym/misc.go` — `--doctor` reports the active provider.
- `cmd/jym-eval/main.go` — eval tool honours the provider selection.
- `README.md` — OpenJEV note and updated key resolution order.

## Provider selection rule

1. `JEV_PROVIDER=openjev` forces OpenJEV (uses `OPENJEV_API_KEY`).
2. `TYPESAFE_API_KEY` env var or credentials file → TypeSafe (unchanged default).
3. `OPENJEV_API_KEY` only → OpenJEV.

## Configuration

Set `OPENJEV_API_KEY` (from https://openjev.sh/dashboard) or
`JEV_PROVIDER=openjev` + `OPENJEV_API_KEY` to use OpenJEV. No other changes
are needed. `JYM_API_ENDPOINT` still overrides the endpoint for testing.

## Verification

A live POST to `https://api.openjev.sh/v1/systemone` with model `openjev`,
state `ping`, and one noul question returned HTTP 200. No `api.typesafe.ai`
default was changed — it remains the default endpoint when a TypeSafe key
is present.

## Upstream

Original project: https://github.com/syumai/jevyoumean by @syumai.
