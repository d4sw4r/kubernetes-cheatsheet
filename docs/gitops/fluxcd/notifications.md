---
tags: [flux, alerting]
---
# Notifications

Der notification-controller schickt Events von Flux-Objekten an Chat-Tools (Discord, Slack, Teams, …).

## Discord

```yaml title="snippets/flux/notifications-discord.yaml"
--8<-- "snippets/flux/notifications-discord.yaml"
```

Das Secret mit der Webhook-URL (vor dem Commit mit SOPS verschlüsseln, siehe [SOPS](sops.md)):

```bash
kubectl -n flux-system create secret generic discord-webhook-secret \
  --from-literal=address=https://discord.com/api/webhooks/<id>/<token> \
  --dry-run=client -o yaml > secrets/my-cluster/discord-webhook-secret.yaml
sops --encrypt --in-place secrets/my-cluster/discord-webhook-secret.yaml
```

## Quick commands

```bash
flux get alerts -A
flux get alert-providers -A
kubectl -n flux-system logs deploy/notification-controller
```

## Gotchas / Learnings

- `eventSeverity: info` meldet jeden erfolgreichen Reconcile – sehr gesprächig. Für Produktion `error` nehmen oder einen zweiten Alert mit `info` nur für HelmRelease.
- `name: "*"` deckt alle Objekte einer Art ab, aber nur im **Namespace des Alerts** – für andere Namespaces `namespace:` in `eventSources` angeben.
- `eventMetadata.cluster` hilft, wenn mehrere Cluster in denselben Kanal posten.
