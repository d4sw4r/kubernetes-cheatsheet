---
tags: [cilium, networking, gateway-api]
---
# Cilium & Gateway API

**Wofür?** eBPF-basiertes CNI. Bei mir ersetzt es kube-proxy, stellt LoadBalancer-IPs bereit (LB-IPAM + L2-Announcements) und implementiert die **Gateway API** statt eines Ingress-Controllers.

## Installation via Flux

```yaml title="snippets/flux/helm/cilium.yaml"
--8<-- "snippets/flux/helm/cilium.yaml"
```

## Gateway-API-CRDs

Müssen **vor** Cilium (mit `gatewayAPI.enabled=true`) installiert sein:

```bash
V=v1.2.0   # zur Cilium-Version passende Gateway-API-Version (siehe Cilium-Doku)
for crd in gatewayclasses gateways httproutes referencegrants grpcroutes; do
  kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/gateway-api/$V/config/crd/standard/gateway.networking.k8s.io_$crd.yaml
done
kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/gateway-api/$V/config/crd/experimental/gateway.networking.k8s.io_tlsroutes.yaml
```

Wurde Cilium vorher installiert, Operator und Agents neu starten:

```bash
kubectl -n kube-system rollout restart deployment/cilium-operator
kubectl -n kube-system rollout restart ds/cilium
```

## LoadBalancer-IP-Pool

```yaml title="snippets/networking/cilium-lb-pool.yaml"
--8<-- "snippets/networking/cilium-lb-pool.yaml"
```

## Gateway mit TLS (cert-manager)

```yaml title="snippets/networking/gateway-tls.yaml"
--8<-- "snippets/networking/gateway-tls.yaml"
```

## HTTPRoute

```yaml title="snippets/networking/httproute.yaml"
--8<-- "snippets/networking/httproute.yaml"
```

## Redirect HTTP → HTTPS

```yaml title="snippets/networking/httproute-redirect.yaml"
--8<-- "snippets/networking/httproute-redirect.yaml"
```

## Quick commands

```bash
cilium status
cilium connectivity test
kubectl get gateway,httproute -A
kubectl get ciliumloadbalancerippool
hubble observe --namespace default        # Flows live (Hubble Relay nötig)
```

## Gotchas / Learnings

- **Talos:** `cni.name: none` bzw. `custom` und `proxy.disabled: true` setzen, `cgroup.autoMount.enabled=false` + `hostRoot=/sys/fs/cgroup`, eingeschränkte `capabilities` (siehe Snippet).
- **Ein Listener pro Hostname** im Gateway – cert-manager (`enableGatewayAPI: true`) erzeugt dann pro Listener das Zertifikat aus `certificateRefs`.
- **`nip.io`** ist praktisch für Tests ohne eigene Domain (`grafana.<IP>.nip.io`).
- `HTTPRoute` ist seit Gateway API v1.0 in `gateway.networking.k8s.io/v1` – ältere Beispiele mit `v1beta1` umstellen.

## Quellen

- [Cilium Gateway API](https://docs.cilium.io/en/stable/network/servicemesh/gateway-api/gateway-api/)
- [cilium-helm-template](https://github.com/d4sw4r/cilium-helm-template) – gerendertes Manifest für Talos `cni.urls`
