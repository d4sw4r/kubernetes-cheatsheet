---
tags: [metallb, networking]
---
# MetalLB

**Wofür?** `type: LoadBalancer` auf Bare Metal oder VMs ohne Cloud-Loadbalancer.

```bash
helm repo add metallb https://metallb.github.io/metallb
helm install metallb metallb/metallb -n metallb-system --create-namespace
```

```yaml title="snippets/networking/metallb-pool.yaml"
--8<-- "snippets/networking/metallb-pool.yaml"
```

!!! tip
    Ohne `L2Advertisement` (oder `BGPAdvertisement`) vergibt MetalLB zwar IPs, kündigt sie aber nicht im Netz an.

Alternative mit Cilium: [LB-IPAM + L2-Announcements](cilium.md#loadbalancer-ip-pool).
