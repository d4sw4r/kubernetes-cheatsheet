---
tags: [flux, debugging]
---
# Debugging

```bash
flux get all -A --status-selector ready=false     # Nur was nicht Ready ist
flux events -A                                     # Events aller Flux-Objekte
flux logs --kind=Kustomization --name=apps -n flux-system
kubectl -n flux-system get pods                    # Laufen die Controller?
flux trace deployment my-app -n default            # Welche Kustomization/Source verwaltet das?
```

| Symptom | Ansatz |
|---|---|
| `dependency ... is not ready` | Abhängige Kustomization zuerst prüfen |
| `health check failed` | `wait`/`timeout` – betroffene Ressource mit `kubectl describe` anschauen |
| HelmRelease hängt in `upgrade retries exhausted` | `flux suspend` + `flux resume`, ggf. `helm rollback` |
| Source `authentication required` | Secret / Deploy-Key prüfen |

## Gotchas / Learnings

!!! note "TODO"
    Eigene Fälle ergänzen.
