---
applyTo: "**/pt-pneuma*/**"
---

# Pneuma Team Instructions

## GitHub Actions

Pneuma's GKE clusters are regional and their node pools are zonal. Each deployment workspace is named for a zone (`us-east1-b`, `us-east4-a`), and workflow job names call them "Regional". Each zonal workflow starts after `main` (every job lists `main` in `needs:`), but some jobs have cross-zone dependencies, so zones are not fully independent (see below).

Zone-scoped deployments live under `regional/`; environment-wide API configuration lives under `config/authentik`, `config/datadog`, and `config/openbao`. After the zonal workspace, these workspaces deploy in the following order. Treat the workflow `needs:` graph as the source of truth and keep this list in sync with it:

1. `onboarding`
2. `cert-manager`, `openbao`, and `datadog` (all after `onboarding`)
3. cert-manager Istio CSR (after `cert-manager`), then `istio`
4. `authentik` (after `istio`; `us-east4-a` also waits for `us-east1-b`), then Istio manifests, then `config/authentik` (after both zones' Istio manifests)
5. `config/openbao` (after `openbao`, `config/authentik`, and `main`); the global OpenBao hostname routes only to `us-east1-b`
6. Datadog manifests (after `datadog`); `opa-gatekeeper`, then its templates, then its constraints (after `cert-manager`)
7. Istio test (after Istio manifests)
8. `config/datadog` (after all Istio Test workspaces, `config/authentik`, and `config/openbao`)
