# Inbox

Schnell notieren, später einsortieren. Ideal für Befehle aus der Shell-History:

```bash
history | grep -E 'kubectl|flux|helm|pluto|kubescape|kuberadar' | sort -u -k2
```

## Offen

- [ ] Flux Image Automation (ImageRepository / ImagePolicy / ImageUpdateAutomation) dokumentieren
- [ ] Flux Notifications (Provider / Alert) dokumentieren
- [ ] Snippets aus den Projekten in `~/git/` einsammeln
