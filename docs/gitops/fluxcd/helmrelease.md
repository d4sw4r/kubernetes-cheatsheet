---
tags: [flux, helm]
---
# HelmRelease

## HelmRepository

```yaml title="snippets/flux/helmrepository.yaml"
--8<-- "snippets/flux/helmrepository.yaml"
```

## HelmRelease

```yaml title="snippets/flux/helmrelease.yaml"
--8<-- "snippets/flux/helmrelease.yaml"
```

## Quick commands

```bash
flux get helmreleases -A
flux reconcile helmrelease redis -n redis --with-source
helm history redis -n redis                     # Flux nutzt normale Helm-Releases
kubectl get helmrelease redis -n redis -o yaml  # Status & Conditions
```

## Gotchas / Learnings

!!! note "TODO"
    Eigene Erfahrungen ergänzen.
