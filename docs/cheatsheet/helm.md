---
tags: [helm]
---
# Helm

Für Cluster mit Flux laufen Charts als [HelmRelease](../gitops/fluxcd/helmrelease.md). Die Befehle hier sind für Lab-Cluster, zum Ausprobieren und zum Debuggen.

## Installation

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
# oder
brew install helm
```

## Repos & Charts finden

```bash
helm repo add gitea-charts https://dl.gitea.com/charts/
helm repo update
helm search repo gitea                       # Charts im Repo suchen
helm search repo gitea/gitea --versions      # alle Versionen
helm show values gitea-charts/gitea > values.yaml   # Default-Values als Startpunkt
```

## Installieren & Updaten

```bash
helm install gitea gitea-charts/gitea -n gitea --create-namespace
helm upgrade gitea gitea-charts/gitea -n gitea -f values.yaml       # geänderte Values ausrollen
helm upgrade --install gitea gitea-charts/gitea -n gitea --create-namespace -f values.yaml --version <x.y.z>   # idempotent, ideal für Skripte
```

## Status & Debugging

```bash
helm list -A                                 # alle Releases
helm status gitea -n gitea
helm get values gitea -n gitea               # nur die eigenen Values
helm get values gitea -n gitea --all         # inkl. Defaults
helm get manifest gitea -n gitea             # was wirklich installiert wurde
helm template gitea gitea-charts/gitea -f values.yaml > rendered.yaml   # lokal rendern, ohne Cluster
```

## Rollback & Entfernen

```bash
helm history gitea -n gitea
helm rollback gitea 3 -n gitea               # auf Revision 3 zurück
helm uninstall gitea -n gitea
```

## Gotchas / Learnings

- Nach `helm install` fehlt oft noch der Zugriff von außen – Ingress bzw. HTTPRoute im **selben Namespace** wie der Service anlegen (oder per Chart-Values aktivieren).
- Sobald `helm upgrade` ein `-f` oder `--set` bekommt, gelten nur diese Werte plus Chart-Defaults – früher per `--set` gesetzte Werte sind dann weg. Deshalb immer die komplette `values.yaml` mitgeben (oder `--reuse-values` bewusst setzen).
- Releases, die Flux verwaltet, nicht per Hand mit `helm upgrade` ändern – Flux überschreibt das beim nächsten Reconcile.
- `helm template` + [Pluto](../tools/pluto.md) / [Kubescape](../tools/kubescape.md) prüft ein Chart, bevor es im Cluster landet.
