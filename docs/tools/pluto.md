---
tags: [tool, upgrade]
---
# Pluto

**Wofür?** Findet `apiVersion`s, die in einer Ziel-Kubernetes-Version deprecated oder entfernt sind – in Dateien, Helm-Releases und im laufenden Cluster.

## Installation

```bash
brew install FairwindsOps/tap/pluto
# oder
go install github.com/fairwindsops/pluto/v5/cmd/pluto@latest
```

## Quick commands

```bash
pluto detect-files -d ./manifests                 # Lokale Manifeste prüfen
pluto detect-helm -owide                           # Installierte Helm-Releases prüfen
pluto detect-api-resources -owide                  # Laufende Ressourcen im Cluster prüfen
pluto detect-files -d . --target-versions k8s=v1.33.0   # Gegen bestimmte Zielversion prüfen
pluto list-versions                                # Bekannte Deprecations anzeigen
```

!!! info "In der CI"
    Dieses Repo prüft `snippets/` bei jedem Push mit `pluto detect-files` (siehe `.github/workflows/lint.yml`).
    Exit-Code ≠ 0 bei Funden – mit `--ignore-deprecations` / `--ignore-removals` steuerbar.

## Gotchas / Learnings

- Vor jedem Cluster-Upgrade das GitOps-Repo scannen: `pluto detect-files -d .` im Root des Flux-Repos.
- `detect-files` findet nur, was als Datei vorliegt – Charts, die Flux rendert, prüft `detect-helm` im Cluster.

## Quellen

- [Pluto Doku](https://pluto.docs.fairwinds.com/)
