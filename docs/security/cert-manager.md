---
tags: [cert-manager, security, tls]
---
# cert-manager

## Installation via Flux

```yaml title="snippets/flux/helm/cert-manager.yaml"
--8<-- "snippets/flux/helm/cert-manager.yaml"
```

## Eigene CA (Lab / intern)

```yaml title="snippets/security/cert-manager-selfsigned-ca.yaml"
--8<-- "snippets/security/cert-manager-selfsigned-ca.yaml"
```

Ein `Gateway` mit der Annotation `cert-manager.io/issuer: ca-issuer` bekommt dann automatisch Zertifikate – siehe [Cilium & Gateway API](../networking/cilium.md#gateway-mit-tls-cert-manager).

## Quick commands

```bash
kubectl get certificate,certificaterequest,issuer,clusterissuer -A
kubectl describe certificate <name>
cmctl status certificate <name>     # cert-manager CLI
cmctl renew <name>
```

## Gotchas / Learnings

- Ein `Issuer` ist namespaced – für mehrere Namespaces einen `ClusterIssuer` nehmen.
- Für Gateway-API-Support muss `enableGatewayAPI: true` in der Controller-Config gesetzt sein **und** die Gateway-CRDs müssen vor cert-manager existieren.
- Let's Encrypt mit k3s: [k3s.rocks – cert-manager](https://k3s.rocks/https-cert-manager-letsencrypt/)
