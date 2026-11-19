---
tags: [traefik, networking, k3s]
---
# Traefik

**Wofür?** Ingress-Controller (Default in k3s) mit eigenen CRDs (`IngressRoute`, `IngressRouteTCP`, `IngressRouteUDP`).

## Dashboard

```yaml title="snippets/networking/traefik-dashboard.yaml"
--8<-- "snippets/networking/traefik-dashboard.yaml"
```

## Neuen TCP-/UDP-Listener hinzufügen

1. **entryPoint** in Traefik anlegen (k3s: per `HelmChartConfig`):

    ```yaml title="snippets/networking/k3s-traefik-helmchartconfig.yaml"
    --8<-- "snippets/networking/k3s-traefik-helmchartconfig.yaml"
    ```

2. **IngressRouteUDP** (bzw. `IngressRouteTCP`) deployen:

    ```yaml title="snippets/networking/traefik-ingressroute-udp.yaml"
    --8<-- "snippets/networking/traefik-ingressroute-udp.yaml"
    ```

3. Prüfen, ob der **LoadBalancer-Service** von Traefik den Port wirklich öffnet:

    ```bash
    kubectl -n kube-system get svc traefik -o yaml | grep -A5 ports
    ```

## Gotchas / Learnings

- Ohne `expose` im Chart wird der Port zwar als entryPoint angelegt, aber nicht am Service geöffnet. Manuell nachziehen (k9s / `kubectl edit`) geht, ist aber beim nächsten Helm-Upgrade wieder weg.
- Traefik v3: API-Gruppe ist `traefik.io/v1alpha1`, das alte `traefik.containo.us` ist entfernt.
