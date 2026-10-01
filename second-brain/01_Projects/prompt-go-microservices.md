# Prompt: go-order-manager (producao simulada, 100% local)

Quero aprender Go e microservices simulando um ambiente de producao, tudo local com Docker (sem custo de cloud). Os repos msdpramod/* sao apenas estudos de HLD, so com README e sem codigo.

Leia o `CLAUDE.md` da raiz (inclui a golden rule: repo publico, nunca commitar segredos) e o da pasta em que for mexer antes de qualquer coisa.

## Estrutura (monorepo unico, publico)
- `services/`: ratelimiter, gateway, order, inventory, payment.
- `pkg/`, `proto/`, `deploy/compose/`, `loadtest/`.
- `k8s/`: kind ou k3d sobre Docker, Helm, ArgoCD, observabilidade.
- `migrations/`: SQL por servico.
- `second-brain/`: notas, decisoes, runbooks, postmortems.

## Regras
- pt-BR nas respostas. Codigo e commits em ingles. Commits Conventional, uma linha, sem body.
- Sem comentarios no codigo. Sem push ou PR sem minha autorizacao.
- Plano antes de implementar (fatos, suposicoes, essencial vs fase 2) e espere EXECUTE_PLAN.
- Explique o porque das decisoes de Go e de plataforma.

## Etapas
0. Base de producao: Docker Compose, cluster kind ou k3d com ArgoCD, CI (GitHub Actions: lint, test, build, scan), `pkg/` com health, graceful shutdown e telemetria basica.
1. Rate Limiter (README: github.com/msdpramod/Distributed-Rate-Limiter). Fase 1: fixed window, sliding window counter e token bucket atras de uma interface, middleware HTTP com 429 e headers RateLimit-*, token bucket em Redis com Lua atomico, teste de carga com k6. Fase 2: fail-open/closed por endpoint, limiter local de emergencia, circuit breaker, metricas, teste de caos (Redis lento ou fora). Hot key e multi-region: so estudo.
2. API Gateway: reverse proxy, autenticacao simples, usa o limiter, roteamento, tracing ponta a ponta.
3. Order Management: order, inventory, payment com banco proprio cada, gRPC e depois eventos, saga com idempotencia e compensacao, SLOs e alertas.

A cada incidente simulado, registrar runbook e postmortem no vault.

## Decisoes em aberto
- Observabilidade: SigNoz ou Grafana stack.
- Mensageria: NATS ou Kafka.

## Como comecar
1. Leia este arquivo e o CLAUDE.md de cada repo.
2. Proponha o plano da etapa 0 (sem codigo) e aguarde minha aprovacao.
