---
tags: [kubeadm]
---
# kubeadm

!!! info "Historisch"
    Mein Setup stammt aus CentOS-7-Zeiten. Die Paketquellen unter `packages.cloud.google.com` sind abgeschaltet – heute `pkgs.k8s.io` verwenden.

## Node vorbereiten

```bash
# Swap aus
sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
swapoff -a

# Kernel-Module
tee /etc/modules-load.d/containerd.conf <<EOF
overlay
br_netfilter
EOF
modprobe overlay
modprobe br_netfilter

# sysctl
tee /etc/sysctl.d/kubernetes.conf <<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF
sysctl --system
```

## Automatisiert per Ansible

Meine Rolle (in [d4sw4r/kubernetes](https://github.com/d4sw4r/kubernetes), Ordner `kubeadm-absible-role/`) erledigt:

1. `prepare` – Swap, Module, sysctl
2. `container_runtime` – containerd
3. `init_cluster` – `kubeadm init`
4. `install_helm`, `install_ingress`, `install_metallb`, `install_metrics`

## Quellen

- [kubeadm installieren](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)
