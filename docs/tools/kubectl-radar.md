---
tags: [tool, dashboard]
---
# kubectl radar

**Wofür?** Lokale Web-UI für Kubernetes ([Radar von Skyhook](https://github.com/skyhook-io/radar)): Topologie-Ansicht, Event-Timeline, Helm-Releases und Status von GitOps-Deployments (FluxCD, ArgoCD). Läuft als Single Binary lokal, ohne Account oder Cloud.

## Installation

```bash
kubectl krew install radar
# Alternativen laut Projekt: Install-Script, Homebrew, Scoop – siehe README
```

## Quick commands

```bash
kubectl radar                         # startet die UI und öffnet den Browser (http://localhost:9280)
kubectl radar --kubeconfig=prod       # gegen einen bestimmten Cluster
```

## Gotchas / Learnings

- Gut geeignet, um Flux-Kustomizations und HelmReleases visuell zu prüfen, ohne `flux get all -A` zu lesen.
- Bringt auch einen MCP-Server mit, damit AI-Assistenten den Cluster abfragen können.

!!! note "TODO"
    Eigene Erfahrungen ergänzen.

## Quellen

- [skyhook-io/radar](https://github.com/skyhook-io/radar)
- [Radar Kubernetes UI (computingforgeeks)](https://computingforgeeks.com/radar-kubernetes-ui/)
