---
tags: [tool, security]
---
# Kubescape

**Wofür?** Security-Scanner für Cluster, Manifeste und Helm-Charts (Frameworks wie NSA-CISA, MITRE ATT&CK, CIS).

## Installation

```bash
brew install kubescape
# oder
curl -s https://raw.githubusercontent.com/kubescape/kubescape/master/install.sh | /bin/bash
```

## Quick commands

```bash
kubescape scan                                    # Aktuellen Cluster scannen
kubescape scan framework nsa                      # Nur NSA-Framework
kubescape scan framework cis-v1.23-t1.0.1         # CIS-Benchmark
kubescape scan control C-0034 -v                  # Einzelnes Control mit Details
kubescape scan snippets/manifests/                # Lokale Manifeste scannen
kubescape scan --format json --output results.json
kubescape scan --compliance-threshold 80          # Fail unter 80 % (CI)
```

## Ausnahmen

```json title="snippets/tools/kubescape-exceptions.json"
--8<-- "snippets/tools/kubescape-exceptions.json"
```

```bash
kubescape scan --exceptions snippets/tools/kubescape-exceptions.json
```

## Gotchas / Learnings

- Lokale Ordner direkt scannen, z. B. `kubescape scan deployment/` – praktisch vor dem Commit.
- In Lab-Clustern (Talos mit `enforce: privileged`) gibt es naturgemäß viele Funde bei `kube-system` / CNI – dafür Ausnahmen pflegen statt die Ergebnisse zu ignorieren.

## Quellen

- [Kubescape Doku](https://kubescape.io/docs/)
