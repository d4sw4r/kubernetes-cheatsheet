---
tags: [poc, talos, flux, cilium]
---
# PoC: Talos auf Hetzner Bare Metal

| | |
|---|---|
| **Datum** | 2026-01 – 2026-08 |
| **Status** | ✅ läuft |
| **Repo** | `d4sw4r/talos-k8s` (privat) |
| **Stack** | Talos v1.12, Cilium 1.18 (Gateway API, kube-proxy-Ersatz), FluxCD, SOPS/age, cert-manager, Longhorn, kube-prometheus-stack, Renovate |

## Ziel

Ein dedizierter Hetzner-Server als Single-Node-Cluster mit Talos, komplett per GitOps verwaltet – ohne Ingress-Controller, nur mit Cilium Gateway API.

## Setup

### 1. Talos auf den Server bringen (Rescue-System)

```bash
# Im Hetzner-Robot "Rescue" aktivieren, rebooten, per SSH als root verbinden
mdadm --stop /dev/md[0-4]                 # Software-RAID auflösen
sfdisk --delete /dev/nvme[0-1]n1
wipefs -a -f /dev/nvme[0-1]n1

wget https://github.com/siderolabs/talos/releases/download/v1.12.2/metal-amd64.raw.zst -O /tmp/talos.raw.zst
zstd -d /tmp/talos.raw.zst
dd if=/tmp/talos.raw of=/dev/nvme0n1 bs=1M status=progress && sync
reboot
```

### 2. Cluster bootstrappen

```bash
talosctl gen config my-cluster https://<NODE-IP>:6443 --config-patch @talos-patch.yaml
talosctl apply-config -f controlplane.yaml -n <NODE-IP> -e <NODE-IP> --insecure
export TALOSCONFIG=./talosconfig
talosctl config endpoint <NODE-IP>
talosctl config node <NODE-IP>
talosctl bootstrap
talosctl kubeconfig ./kubeconfig
```

Patch: [Talos → Machine-Config-Patch](../distributions/talos.md#machine-config-patch).

### 3. Flux + SOPS

```bash
kubectl create secret generic sops-age -n flux-system --from-file=age.agekey=age.agekey
flux bootstrap github --owner=<user> --repository=talos-k8s --path=clusters/my-cluster --personal
```

Danach übernimmt die [Kustomization-Kette](../gitops/fluxcd/layout.md) den Rest.

## Ergebnis

- Cilium übernimmt CNI, kube-proxy, LoadBalancer-IPs (`CiliumLoadBalancerIPPool` mit der Server-IP) und Gateway API.
- Hubble UI, Grafana und Longhorn laufen hinter einem `Gateway` mit TLS über eine eigene CA (cert-manager).
- Flux meldet alle Reconciles in einen Discord-Kanal.

## Node zurücksetzen

```bash
talosctl reset --system-labels-to-wipe EPHEMERAL,STATE --reboot -n <NODE-IP> --graceful=false
```

Falls der Node danach nicht mehr bootet: Rescue aktivieren, Hardware-Reset, Image erneut per `dd` schreiben, mit `talosctl get disks --insecure` prüfen, Config wieder anwenden und neu bootstrappen.

## Learnings / Gotchas

- **Hetzner-RAID:** Vorinstallierte Server haben ein mdadm-RAID – vorher stoppen und Disks wipen, sonst bootet Talos nicht sauber.
- **Longhorn braucht ein Factory-Image** mit `iscsi-tools`; beim Upgrade auf Single-Node unbedingt `--preserve`.
- **Gateway-API-CRDs vor Cilium**, sonst wird kein `GatewayClass cilium` angelegt – danach Operator neu starten.
- **CNI beim Bootstrap:** Cilium wird einmal als gerendertes Manifest über `cni.urls` installiert ([cilium-helm-template](https://github.com/d4sw4r/cilium-helm-template)) und anschließend von Flux als HelmRelease übernommen.

## Wiederverwendbare Snippets

- [Kustomization-Kette](../gitops/fluxcd/layout.md)
- [Discord-Notifications](../gitops/fluxcd/notifications.md)
- [HelmReleases: cert-manager, Longhorn, kube-prometheus-stack, Cilium](../gitops/fluxcd/helmrelease.md#beispiele-aus-meinen-clustern)
- [Gateway, HTTPRoute, Redirect, LB-Pool](../networking/cilium.md)
- [cert-manager mit eigener CA](../security/cert-manager.md)
