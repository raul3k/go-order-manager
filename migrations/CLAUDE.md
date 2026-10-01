# migrations

Versioned SQL per service for go-order-manager (one database per service). Root `CLAUDE.md` rules apply.

## Layout
`<service>/NNNN_description.sql`, one folder per service.

## Rules
- Migration lands and is applied BEFORE the code that depends on it.
- Schema changes must be zero-downtime friendly.
