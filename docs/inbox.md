# Inbox

Schnell notieren, später einsortieren. Ideal für Befehle aus der Shell-History:

```bash
history | grep -E 'kubectl|flux|helm|talosctl|cilium|pluto|kubescape' | sort -u -k2
```

## Offen

- [ ] Flux Image Automation (ImageRepository / ImagePolicy / ImageUpdateAutomation) dokumentieren
- [ ] Renovate für Flux-Repos dokumentieren (`renovate.json` aus `talos-k8s`)
- [ ] Ergebnis des llmkube-PoCs ergänzen
- [ ] Eigene Erfahrungen zu kubectl radar ergänzen
- [ ] Helm-Notizen aus `d4sw4r/kubernetes` (Gitea, Drone CI) einsortieren oder verwerfen
- [x] Flux Notifications dokumentieren
- [x] Snippets aus den eigenen Repos einsammeln
