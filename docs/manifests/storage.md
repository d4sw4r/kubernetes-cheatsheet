---
tags: [manifest, storage]
---
# Storage

## PersistentVolume (hostPath)

```yaml title="snippets/manifests/pv.yaml"
--8<-- "snippets/manifests/pv.yaml"
```

## PersistentVolumeClaim

```yaml title="snippets/manifests/pvc.yaml"
--8<-- "snippets/manifests/pvc.yaml"
```

!!! warning "Gotcha"
    `hostPath` bindet an einen Node – nur für Single-Node/Lab-Cluster geeignet.
