# Kubernetes Knowledge Base

Persönliche Wissensdatenbank zu Kubernetes, FluxCD und Tooling (Pluto, Kubescape, kuberadar, …) – gebaut mit [MkDocs Material](https://squidfunk.github.io/mkdocs-material/).

- **`docs/`** – Markdown-Seiten
- **`snippets/`** – echte, valide Manifeste; werden per `--8<-- "snippets/..."` in die Seiten eingebunden und in der CI mit `yamllint`, `kubeconform` und `pluto` geprüft

## Lokal

```bash
pip install -r requirements.txt
mkdocs serve          # http://127.0.0.1:8000
```

## Neue Inhalte

| Was | Wohin |
|---|---|
| Schnelle Notiz | `docs/inbox.md` |
| Manifest / Flux-Objekt | Datei in `snippets/…`, auf einer Seite einbinden |
| Tool | `docs/tools/kuberadar.md` als Vorlage kopieren |
| PoC | `docs/pocs/_template.md` kopieren |

Neue Seiten in `mkdocs.yml` unter `nav` eintragen.

## Deployment

Push auf `main` baut die Seite und veröffentlicht sie auf GitHub Pages
(einmalig: *Settings → Pages → Source: GitHub Actions*).
