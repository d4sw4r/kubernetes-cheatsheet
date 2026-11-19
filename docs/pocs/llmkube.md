---
tags: [poc, ai, flux]
---
# PoC: LLM-Inference mit llmkube

| | |
|---|---|
| **Datum** | 2026-08 |
| **Status** | 🚧 in Arbeit |
| **Repo** | `d4sw4r/ai-cluster` (privat) |
| **Stack** | FluxCD, [LLMKube](https://github.com/defilantech/LLMKube)-Operator, llama.cpp, Qwen3-8B (GGUF), NVIDIA GPU |

## Ziel

LLMs deklarativ als Kubernetes-Ressourcen betreiben: Modell und Inference-Service per GitOps statt per Hand.

## Setup

Operator per Flux:

```yaml title="snippets/pocs/llmkube/llmkube.yaml"
--8<-- "snippets/pocs/llmkube/llmkube.yaml"
```

Modell (wird von Hugging Face geladen):

```yaml title="snippets/pocs/llmkube/model.yaml"
--8<-- "snippets/pocs/llmkube/model.yaml"
```

Inference-Service, der das Modell mit llama.cpp ausliefert:

```yaml title="snippets/pocs/llmkube/inferenceservice.yaml"
--8<-- "snippets/pocs/llmkube/inferenceservice.yaml"
```

## Ergebnis

!!! note "TODO"
    Ergebnis ergänzen (Tokens/s, GPU-Auslastung, Erfahrungen mit dem Operator).

## Learnings / Gotchas

- `Model` und `InferenceService` sind CRDs von llmkube – kubeconform kann sie mangels Schema nicht prüfen (in der CI per `-skip` ausgenommen).
- `memoryBudget` muss zur GPU passen (hier 24 Gi für ein Q4-quantisiertes 8B-Modell).
