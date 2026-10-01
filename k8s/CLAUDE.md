# k8s

Local platform for go-order-manager. No cloud: a kind or k3d cluster running on Docker. Root `CLAUDE.md` rules apply.

## Layout
- `clusters/local`: cluster definition and bootstrap.
- `apps`: ArgoCD applications (GitOps source of truth, pointing at paths in this repo).
- `charts`: Helm charts for the services.
- `observability`: OpenTelemetry collector and backend config.

## Rules
- The cluster state comes from this repo via ArgoCD. Do not apply manifests by hand except for bootstrap.
- Confirm a change is merged in `origin/main` before calling it the deployed state.
