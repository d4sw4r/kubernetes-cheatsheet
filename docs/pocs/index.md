# PoCs

Experimente und Proof-of-Concepts. Der Code bleibt im jeweiligen Projekt-Repo – hier stehen Ziel, Ergebnis und Learnings.

| PoC | Thema | Status | Repo |
|---|---|---|---|
| [Talos auf Hetzner Bare Metal](talos-hetzner.md) | Talos, Cilium Gateway API, FluxCD, SOPS | ✅ läuft | `d4sw4r/talos-k8s` |
| [LLM-Inference mit llmkube](llmkube.md) | llmkube-Operator, Qwen3, GPU | 🚧 in Arbeit | `d4sw4r/ai-cluster` |
| [Prometheus Operator mit Kustomize](prometheus-operator.md) | Monitoring ohne Helm | ✅ abgeschlossen | [`d4sw4r/prometheus-operator-example`](https://github.com/d4sw4r/prometheus-operator-example) |

!!! tip "Neuen PoC anlegen"
    [`_template.md`](_template.md) kopieren, ausfüllen, in `mkdocs.yml` unter `PoCs` eintragen.
