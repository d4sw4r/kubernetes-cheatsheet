---
tags: [poc, monitoring, kustomize]
---
# PoC: Prometheus Operator mit Kustomize

| | |
|---|---|
| **Datum** | 2024-03 |
| **Status** | ✅ abgeschlossen |
| **Repo** | [d4sw4r/prometheus-operator-example](https://github.com/d4sw4r/prometheus-operator-example) · [Blog](https://d4sw4r.github.io) |
| **Stack** | prometheus-operator, Prometheus, Alertmanager, Blackbox-Exporter, Kustomize |

## Ziel

Ein Gerüst für Prometheus mit dem Operator **ohne Helm**, nur mit Kustomize – dazu gab es kaum Beispiele.

## Setup

```bash
kubectl create -k bootstrap/    # CRDs (create statt apply: die CRDs sind zu groß für die last-applied-Annotation)
kubectl apply -k base/          # Operator, Prometheus, Alertmanager, Blackbox
```

```yaml title="snippets/pocs/prometheus-operator/kustomization.yaml"
--8<-- "snippets/pocs/prometheus-operator/kustomization.yaml"
```

## Kernidee: ein Label für alles

```yaml title="snippets/pocs/prometheus-operator/prometheus.yaml"
--8<-- "snippets/pocs/prometheus-operator/prometheus.yaml"
```

Jede `ScrapeConfig`, `PrometheusRule`, `ServiceMonitor` und `Probe` mit `prometheus: prometheus` wird automatisch geladen:

=== "ScrapeConfig"

    ```yaml title="snippets/pocs/prometheus-operator/scrapeconfig.yaml"
    --8<-- "snippets/pocs/prometheus-operator/scrapeconfig.yaml"
    ```

=== "PrometheusRule"

    ```yaml title="snippets/pocs/prometheus-operator/prometheusrule.yaml"
    --8<-- "snippets/pocs/prometheus-operator/prometheusrule.yaml"
    ```

## Learnings / Gotchas

- **Leere Selektoren** (`{}`) laden alles aus allen Namespaces, **fehlende** Selektoren gar nichts – explizites Label ist am übersichtlichsten.
- `ScrapeConfig` (v1alpha1) erlaubt statische Targets ohne Service – ideal für externe Systeme.
- Image-Tags in der Kustomization pinnen statt `latest`.
- Für die meisten Cluster ist heute `kube-prometheus-stack` per [HelmRelease](../gitops/fluxcd/helmrelease.md#beispiele-aus-meinen-clustern) der schnellere Weg.
