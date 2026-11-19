---
tags: [k3s]
---
# k3s

**Wofür?** Leichtgewichtige Distribution (Single Binary) mit Traefik, ServiceLB und local-path-Storage out of the box.

## Installation

```bash
# API-Server nur auf localhost binden
curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="--bind-address 127.0.0.1" sh -s -

mkdir -p ~/.kube && cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
```

## Mitgelieferte Komponenten anpassen

Alles, was in `/var/lib/rancher/k3s/server/manifests/` liegt, wendet k3s automatisch an. Für die mitgelieferten Helm-Charts (z. B. Traefik) gibt es `HelmChartConfig`:

```yaml title="snippets/networking/k3s-traefik-helmchartconfig.yaml"
--8<-- "snippets/networking/k3s-traefik-helmchartconfig.yaml"
```

Mehr dazu: [Traefik](../networking/traefik.md).

## Private Registry / Mirror

```yaml title="snippets/config/k3s-registries.yaml"
--8<-- "snippets/config/k3s-registries.yaml"
```

## Cluster in der Hetzner Cloud: hetzner-k3s

[hetzner-k3s](https://github.com/vitobotta/hetzner-k3s) erstellt Server, Netzwerk, Firewall, Cloud Controller Manager und CSI-Treiber in wenigen Minuten.

```yaml title="snippets/config/hetzner-k3s-cluster.yaml"
--8<-- "snippets/config/hetzner-k3s-cluster.yaml"
```

!!! warning "Gotcha"
    `ssh_allowed_networks` / `api_allowed_networks` auf `0.0.0.0/0` sind nur für Tests ok.

## Gotchas / Learnings

- StatefulSets mit nur einem Headless-Service sind unpraktisch für extern erreichbare Instanzen – dann lieber ein Helm-Chart nutzen oder zusätzliche Services anlegen.
- Datenbanken in k3s: Resource-Limits sauber setzen, sonst greift schnell der OOM-Killer.
