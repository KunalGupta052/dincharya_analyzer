# Dinacharya Analyzer

A Node.js/Express API that analyzes a user's daily routine (dinacharya) using Claude
(Anthropic's API) and returns structured wellness insights. Built with production-style
practices — health checks, structured logging, rate limiting, and a full test suite —
and packaged with Docker for consistent local and deployed environments.

**Live demo:** https://dincharya-analyzer.onrender.com/

## What's actually running

- **Backend:** Node.js + Express, calling the Anthropic API directly for analysis
- **Security & reliability:** Helmet security headers, CORS, rate limiting on `/api/`
  routes, structured JSON logging, and `/health` / `/ready` endpoints suitable for
  container orchestration health checks
- **Testing:** Jest + Supertest, covering health/readiness endpoints, input validation,
  security headers, and CORS — run with `npm test`
- **Containerization:** Multi-stage Dockerfile (non-root user, health check, small final
  image) — this is genuinely built and used for the deployed version
- **CI:** GitHub Actions runs the full test suite on every push and pull request

## Infrastructure-as-code (design exercises, not currently deployed)

This repo also includes Kubernetes manifests (`infrastructure/k8s/`) and Terraform
configuration (`infrastructure/terraform/`) for an AWS EC2 deployment, plus a
Prometheus/Grafana monitoring stack (`monitoring/`, `docker-compose.yml`).

**These were built to learn and demonstrate infrastructure-as-code patterns — they are
not currently applied to a live cluster or cloud account.** The Kubernetes and Terraform
files contain placeholder values (registry usernames, domains, keys) that would need to
be filled in before actually deploying. I'm including them because writing correct,
production-shaped IaC is something I wanted hands-on practice with, not because this
project runs on them today. The application itself is deployed simply, via Render,
directly from this repo's `main` branch.

## Run locally

\`\`\`bash
npm install
cp .env.example .env   # add your own ANTHROPIC_API_KEY
npm start
\`\`\`

Visit `http://localhost:3000`.

## Run the tests

\`\`\`bash
npm test
\`\`\`

## What I'd improve next

- Persist analysis history to a real database instead of keeping everything stateless
  per-request
- Actually deploy the Kubernetes manifests to a real cluster (kind/minikube locally,
  or a managed cluster) to validate them beyond just reading correctly
- Add integration tests that exercise the real `/api/analyze` flow against a mocked
  Anthropic API response, not just input-validation tests
