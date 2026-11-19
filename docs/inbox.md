# Inbox

Schnell notieren, später einsortieren. Ideal für Befehle aus der Shell-History:

```bash
history | grep -E 'kubectl|flux|helm|talosctl|cilium|pluto|kubescape' | sort -u -k2
```

## Offen

- [ ] talos-k8s: Renovate-Pattern auf `infra/` erweitern und Chart-Versionen pinnen (siehe [Renovate](gitops/renovate.md#gotchas-learnings))
- [ ] exa_website_go: `config:base` → `config:recommended`
- [ ] Ergebnis des llmkube-PoCs ergänzen
- [ ] Eigene Erfahrungen zu kubectl radar ergänzen
- [ ] Helm-Notizen aus `d4sw4r/kubernetes` (Gitea, Drone CI) einsortieren oder verwerfen
- [x] Flux Notifications dokumentieren
- [x] Flux Image Automation und Renovate dokumentieren
- [x] Snippets aus den eigenen Repos einsammeln
