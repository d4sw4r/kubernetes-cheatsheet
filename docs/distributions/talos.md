---
tags: [talos]
---
# Talos Linux

**Wofür?** Immutable, minimales OS nur für Kubernetes – keine SSH-Shell, alles per API (`talosctl`).

## Quick commands

```bash
talosctl gen config my-cluster https://<NODE-IP>:6443 --config-patch @talos-patch.yaml
talosctl apply-config -f controlplane.yaml -n <NODE-IP> -e <NODE-IP> --insecure   # erste Config (Maintenance-Mode)

export TALOSCONFIG=./talosconfig
talosctl config endpoint <NODE-IP>
talosctl config node <NODE-IP>
talosctl bootstrap                     # etcd initialisieren – nur einmal, auf einem Control-Plane-Node
talosctl kubeconfig ./kubeconfig
talosctl dashboard                     # Live-Übersicht des Nodes

talosctl get disks --insecure -n <NODE-IP>   # Disks im Maintenance-Mode anzeigen
talosctl upgrade --image factory.talos.dev/metal-installer/<schematic-id>:v1.12.2 -m powercycle --preserve
talosctl reset --system-labels-to-wipe EPHEMERAL,STATE --reboot -n <NODE-IP> --graceful=false
```

## Machine-Config-Patch

Single-Node mit Cilium als CNI (kein kube-proxy), Workloads auf dem Control-Plane-Node:

```yaml title="snippets/config/talos-patch.yaml"
--8<-- "snippets/config/talos-patch.yaml"
```

## Gotchas / Learnings

- **Image Factory:** Extensions wie `iscsi-tools` und `util-linux-tools` (nötig für **Longhorn**) gibt es nur über ein eigenes Installer-Image von [factory.talos.dev](https://factory.talos.dev). Die Schematic-ID landet in `machine.install.image`.
- **`--preserve` bei Single-Node-Upgrades**, sonst wird die EPHEMERAL-Partition (inkl. Longhorn-Daten) gelöscht.
- **`rotate-server-certificates: true`** braucht den [kubelet-serving-cert-approver](https://github.com/alex1989hu/kubelet-serving-cert-approver), sonst bleiben die CSRs offen und `metrics-server` / `kubectl logs` scheitern an TLS.
- **CNI über `cni.urls`:** Talos kann beim Bootstrap ein gerendertes Manifest per URL laden. Dafür Cilium mit `helm template` rendern und z. B. in einem eigenen Repo ablegen ([cilium-helm-template](https://github.com/d4sw4r/cilium-helm-template)). Danach übernimmt Flux die HelmRelease.
- **PodSecurity `enforce: privileged`** ist cluster-weit gesetzt, damit Cilium und Longhorn laufen – für Produktion lieber pro Namespace labeln.

## Siehe auch

- [PoC: Talos auf Hetzner Bare Metal](../pocs/talos-hetzner.md)
- [terraform-hcloud-talos](https://github.com/hcloud-talos/terraform-hcloud-talos) – Talos in der Hetzner Cloud per Terraform
- [Talos Doku](https://www.talos.dev/)
