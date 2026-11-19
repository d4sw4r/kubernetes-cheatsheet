---
tags: [renovate, gitops, flux]
---
# Renovate

**Wofür?** Renovate erstellt Pull Requests für neue Versionen von Abhängigkeiten – bei mir für Flux-HelmReleases, Container-Images, Go-Module und npm-Pakete. Läuft als [GitHub App](https://github.com/apps/renovate), die Config liegt als `renovate.json` im Repo.

## Meine Configs

=== "Flux-Cluster (talos-k8s)"

    Flux-Manager auf alle GitOps-Ordner, Minor/Patch automatisch mergen:

    ```json title="snippets/renovate/flux.json"
    --8<-- "snippets/renovate/flux.json"
    ```

=== "Web-App mit Automerge (blokhouse)"

    Alles außer Major automatisch mergen, devDependencies immer:

    ```json title="snippets/renovate/automerge.json"
    --8<-- "snippets/renovate/automerge.json"
    ```

=== "Go-Projekt (pocketbase-poc)"

    Nach Updates `go mod tidy` ausführen:

    ```json title="snippets/renovate/go.json"
    --8<-- "snippets/renovate/go.json"
    ```

=== "Minimal"

    Für alles andere (vergleich-app, fam-chat, d4sw4r.github.io):

    ```json title="snippets/renovate/minimal.json"
    --8<-- "snippets/renovate/minimal.json"
    ```

## Was der Flux-Manager erkennt

| Datei-Inhalt | Update |
|---|---|
| `HelmRelease` + `HelmRepository` | `spec.chart.spec.version` |
| `OCIRepository` | `spec.ref.tag` |
| `gotk-components.yaml` | Flux-Version selbst (Header-Kommentar) |
| Container-Images in Manifesten | über den `kubernetes`-Manager (eigenes `managerFilePatterns`) |

## Gotchas / Learnings

- **Nur was im Pattern liegt, wird gefunden.** In `talos-k8s` matchte die Flux-Regel anfangs nur `^clusters/`, die HelmReleases liegen aber unter `infra/` – die wurden so **nie** aktualisiert. Behoben in [talos-k8s#6](https://github.com/d4sw4r/talos-k8s/pull/6): das Pattern deckt jetzt alle GitOps-Ordner ab (siehe Config oben).
- **Ohne `version` kein Update:** HelmReleases ohne `spec.chart.spec.version` kann Renovate nicht pinnen – Flux nimmt dann einfach immer die neueste Version, auch neue Majors. In `talos-k8s` waren das cert-manager, Longhorn und kube-prometheus-stack; seit [talos-k8s#6](https://github.com/d4sw4r/talos-k8s/pull/6) gepinnt.
- **Beim Nachpinnen die aktuelle Version nehmen:** Lief ein Chart ohne `version`, ist im Cluster bereits die neueste Version installiert. Eine ältere Version zu pinnen würde ein Downgrade auslösen – vorher mit `flux get helmreleases -A` prüfen.
- **`fileMatch` → `managerFilePatterns`:** Neuere Renovate-Versionen haben `fileMatch` umbenannt; die Migration schlägt Renovate selbst per PR vor.
- **`config:base` ist veraltet** (noch in `exa_website_go`) – durch `config:recommended` ersetzen.
- **`platformAutomerge: true`** nutzt GitHubs Auto-Merge: Renovate muss nicht erneut laufen, um zu mergen – braucht aber Branch Protection mit Required Checks, sonst wird sofort gemergt.
- Das **Dependency Dashboard** (Issue im Repo) zeigt alle offenen und blockierten Updates – erster Blick, wenn „nichts passiert“.

## Renovate vs. Flux Image Automation

Siehe [Image Automation → Vergleich](fluxcd/image-automation.md#flux-image-automation-vs-renovate).

## Quellen

- [Renovate: Flux-Manager](https://docs.renovatebot.com/modules/manager/flux/)
- [Renovate: Automerge](https://docs.renovatebot.com/key-concepts/automerge/)
