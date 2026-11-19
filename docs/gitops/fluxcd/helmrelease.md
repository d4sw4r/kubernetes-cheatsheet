---
tags: [flux, helm]
---
# HelmRelease

## HelmRepository

```yaml title="snippets/flux/helmrepository.yaml"
--8<-- "snippets/flux/helmrepository.yaml"
```

## HelmRelease

```yaml title="snippets/flux/helmrelease.yaml"
--8<-- "snippets/flux/helmrelease.yaml"
```

## Beispiele aus meinen Clustern

=== "cert-manager"

    ```yaml title="snippets/flux/helm/cert-manager.yaml"
    --8<-- "snippets/flux/helm/cert-manager.yaml"
    ```

=== "Longhorn"

    ```yaml title="snippets/flux/helm/longhorn.yaml"
    --8<-- "snippets/flux/helm/longhorn.yaml"
    ```

=== "kube-prometheus-stack"

    ```yaml title="snippets/flux/helm/kube-prometheus-stack.yaml"
    --8<-- "snippets/flux/helm/kube-prometheus-stack.yaml"
    ```

=== "Cilium"

    ```yaml title="snippets/flux/helm/cilium.yaml"
    --8<-- "snippets/flux/helm/cilium.yaml"
    ```

## Quick commands

```bash
flux get helmreleases -A
flux reconcile helmrelease redis -n redis --with-source
helm history redis -n redis                     # Flux nutzt normale Helm-Releases
kubectl get helmrelease redis -n redis -o yaml  # Status & Conditions
```

## Gotchas / Learnings

- **CRDs aus dem Chart:** cert-manager braucht `crds.enabled: true`, sonst fehlen `Issuer`/`Certificate`.
- **HelmRepository-Namespace:** Die `sourceRef` darf auf einen anderen Namespace zeigen (`namespace: kube-system`) – praktisch, um Repos zentral abzulegen.
- **Longhorn auf Single-Node:** `defaultReplicaCount: 1`, sonst bleiben Volumes `degraded`. Auf Talos werden die Extensions `iscsi-tools` und `util-linux-tools` benötigt.
- **`version` pinnen** (oder ein Range wie `"20.x"`) – sonst landet jedes neue Major-Release ungeprüft im Cluster.
