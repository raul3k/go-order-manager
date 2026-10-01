# go-order-manager

Monorepo for a study project that simulates a production environment in Go, 100% local with Docker (no cloud cost).

## Golden rule: this repo is PUBLIC
- Never commit secrets: env files, API keys, tokens, passwords, private keys, certificates, kubeconfig, cloud credentials. Not even "temporary" or "for local use".
- Config goes in an example file with fake placeholder values. Real values stay in untracked files.
- Never write personal data (real email, phone, addresses, internals from other projects or employers) in files, examples or commit messages.
- Before every commit, review `git diff --staged` for secrets and personal data. If something leaked, stop, tell the user and rotate it. Deleting it in a later commit does not remove it from history.
- Use only the git identity configured for this repo. Never change `user.email` to a personal address.

## Layout
- `services/<name>`: ratelimiter, gateway, order, inventory, payment. Each layered as domain, app, infrastructure, presentation.
- `pkg/`: shared telemetry, config, logging, health and graceful shutdown.
- `proto/`: gRPC contracts.
- `deploy/compose/`: Docker Compose for day-to-day development.
- `loadtest/`: k6 scenarios.
- `k8s/`: local cluster (kind or k3d on Docker), Helm charts, ArgoCD apps, observability. See `k8s/CLAUDE.md`.
- `migrations/`: versioned SQL per service. See `migrations/CLAUDE.md`.
- `second-brain/`: notes, decisions, runbooks, postmortems, prompts (PARA vault). See `second-brain/CLAUDE.md`.

## Rules
- User-facing answers in pt-BR. Code, identifiers and commits in English.
- Commits: Conventional Commits, single line, no body. Base branch: `main`. Never push or open PRs without explicit authorization.
- No code comments. Prefer naming.
- Essentialism: deliver the must-have of each stage, move the rest to phase 2. No speculative abstractions.
- Plan first: separate facts, assumptions and hypotheses, then wait for EXECUTE_PLAN before implementing.
- Domain and app business rules: test-first. Infrastructure and presentation: thin tests after. Run the vet/typecheck gate separately from tests.
- Every service exposes health and readiness endpoints and shuts down gracefully.
- Schema changes land in `migrations/` before the code that uses them. Use separate PRs to practice the ordering.
- Explain the "why" of Go and platform decisions, the goal is learning.
- No paid cloud. Everything runs on Docker (Compose, kind or k3d, LocalStack only if an AWS API is needed).
- CI uses path filters so a change in one area does not run every service.

## Open decisions
- Observability backend: SigNoz or Grafana stack.
- Messaging for stage 3: NATS or Kafka.
