---
applyTo: "**/pt-pneuma*/**"
---

# Pneuma Team Instructions

## GitHub Actions

Pneuma's GKE clusters are regional and their node pools are zonal. Each deployment workspace is named for a zone (`us-east1-b`, `us-east4-a`), and workflow job names call them "Regional". Each zonal workflow starts after `main` (every job lists `main` in `needs:`), but some jobs have cross-zone dependencies, so zones are not fully independent (see below).

After the zonal workspace, the subdirectory workspaces under `regional/` (`agentgateway`, `authentik`, `authentik-config`, `cert-manager`, `datadog`, `istio`, `onboarding`, `opa-gatekeeper`, `openbao`, `openbao-config`) deploy in this order. Treat the workflow `needs:` graph as the source of truth and keep this list in sync with it:

1. `onboarding`
2. `cert-manager`, `openbao`, and `datadog` (all after `onboarding`)
3. cert-manager Istio CSR (after `cert-manager`), then `istio`
4. `authentik` (after `istio`; `us-east4-a` also waits for `us-east1-b`), then Istio manifests, then `authentik-config` (after both zones' Istio manifests)
5. `openbao-config` (after `openbao`, `authentik-config`, and `main`); the global OpenBao hostname routes only to `us-east1-b`
6. Datadog manifests (after `datadog`); `opa-gatekeeper`, then its templates, then its constraints (after `cert-manager`)
7. Istio test (after Istio manifests)

`agentgateway` currently deploys only in `sandbox.yml` (after `istio`; its manifests wait for `authentik-config`).
