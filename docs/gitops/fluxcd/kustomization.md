---
tags: [flux]
---
# Sources & Kustomizations

## GitRepository

```yaml title="snippets/flux/gitrepository.yaml"
--8<-- "snippets/flux/gitrepository.yaml"
```

## Kustomization

```yaml title="snippets/flux/kustomization.yaml"
--8<-- "snippets/flux/kustomization.yaml"
```

| Feld | Bedeutung |
|---|---|
| `prune: true` | Aus Git entfernte Objekte werden auch im Cluster gelöscht |
| `wait: true` | Erst `Ready`, wenn alle Ressourcen healthy sind |
| `dependsOn` | Reihenfolge, z. B. erst CRDs/Infrastruktur, dann Apps |

## Gotchas / Learnings

!!! note "TODO"
    Eigene Erfahrungen ergänzen.
