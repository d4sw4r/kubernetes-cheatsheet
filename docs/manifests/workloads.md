---
tags: [manifest]
---
# Workloads

## ConfigMap

Liefert eine `index.html`, die im Deployment als Volume gemountet wird.

```yaml title="snippets/manifests/configmap.yaml"
--8<-- "snippets/manifests/configmap.yaml"
```

## Deployment

```yaml title="snippets/manifests/deployment.yaml"
--8<-- "snippets/manifests/deployment.yaml"
```

!!! warning "Gotcha"
    `image: nginx:latest` ist für Demos ok – in echten Setups immer eine feste Version (oder Digest) verwenden.

## Image-Pull-Secret

```yaml title="snippets/manifests/docker-registry-secret.yaml"
--8<-- "snippets/manifests/docker-registry-secret.yaml"
```

Im Pod per `spec.imagePullSecrets: [{name: regcred}]` referenzieren.
