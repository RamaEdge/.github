# theedgeworks.ai

**theedgeworks.ai** builds Kubernetes-native edge computing infrastructure for industrial and operational environments.

## What We Build

Our platform enables **safe, deterministic, and offline-tolerant** data ingestion, governance, and streaming from edge devices to local or cloud consumers.

### Core Platform

- **[edgeworks](https://github.com/ramaedge/edgeworks)** — Kubernetes-native edge platform for industrial data (OPC UA, Modbus, MQTT)
- **[edgeworks-sdk](https://github.com/ramaedge/edgeworks-sdk)** — SDK for building protocol adapters

## Design Principles

- **Kubernetes-first** — CRDs as the source of truth, GitOps-friendly
- **Edge-safe** — Offline operation, intermittent connectivity, fail-safe defaults
- **Deterministic** — Declarative intent compiled to immutable snapshots
- **Strict rejection** — Invalid or unsafe configurations never apply
- **Observable** — OpenTelemetry for logs, metrics, and traces

## Getting Started

See the [edgeworks documentation](https://github.com/ramaedge/edgeworks/tree/main/docs) for installation and configuration guides.

## For Contributors

- [Shared Actions Documentation](docs/ACTIONS.md) — Reusable GitHub Actions and workflows for ramaedge repositories
