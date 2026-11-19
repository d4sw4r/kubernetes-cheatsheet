# Kubernetes Knowledge Base

Persönliche Sammlung von Befehlen, Manifesten, GitOps-Setups, Tools und PoCs rund um Kubernetes.

<div class="grid cards" markdown>

- :material-console: **[Cheatsheet](cheatsheet/index.md)** – kubectl-Befehle & Troubleshooting
- :material-file-code: **[Manifeste](manifests/index.md)** – getestete YAML-Snippets
- :material-source-branch: **[FluxCD](gitops/fluxcd/index.md)** – Bootstrap, Kustomizations, HelmReleases, SOPS
- :material-tools: **[Tools](tools/index.md)** – Pluto, Kubescape, kuberadar, …
- :material-flask: **[PoCs](pocs/index.md)** – Experimente & Learnings
- :material-inbox: **[Inbox](inbox.md)** – Schnellnotizen, noch nicht einsortiert

</div>

## Aufbau des Repos

| Ordner | Inhalt |
|---|---|
| `docs/` | Markdown-Seiten (Erklärungen, Gotchas, Quellen) |
| `snippets/` | Echte, valide YAML-/Shell-Dateien – werden per `--8<--` in die Seiten eingebunden und in der CI geprüft |

## Seiten-Raster

Jede Seite folgt grob diesem Schema:

1. **Wofür?** – ein, zwei Sätze
2. **Quick commands** – das, was man in 90 % der Fälle braucht
3. **Snippet** – eingebunden aus `snippets/`
4. **Gotchas / Learnings** – eigene Erfahrungen
5. **Quellen** – Doku-Links, eigene Repos

## Lokal starten

```bash
pip install -r requirements.txt
mkdocs serve
```
