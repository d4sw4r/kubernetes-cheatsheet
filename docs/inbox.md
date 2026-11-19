# Inbox

Schnell notieren, später einsortieren. Ideal für Befehle aus der Shell-History:

```bash
history | grep -E 'kubectl|flux|helm|talosctl|cilium|pluto|kubescape' | sort -u -k2
```

## Offen

- [ ] Ergebnis des llmkube-PoCs ergänzen
- [ ] Eigene Erfahrungen zu kubectl radar ergänzen
- [x] Flux Notifications dokumentieren
- [x] Flux Image Automation und Renovate dokumentieren
- [x] talos-k8s: Renovate-Pattern auf `infra/` erweitert und Chart-Versionen gepinnt ([talos-k8s#6](https://github.com/d4sw4r/talos-k8s/pull/6))
- [x] Snippets aus den eigenen Repos einsammeln
- [x] exa_website_go: `config:base` → `config:recommended` ([exa_website_go#95](https://github.com/d4sw4r/exa_website_go/pull/95))
- [x] Helm-Notizen einsortiert: Gitea als Beispiel auf der [Helm-Seite](cheatsheet/helm.md), Drone-CI-Notiz verworfen (unvollständig)
