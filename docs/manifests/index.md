# Manifeste

Alle Manifeste liegen als eigene Dateien unter `snippets/manifests/` und werden in der CI mit `kubeconform` und `pluto` geprüft.

Das komplette nginx-Beispiel (ConfigMap → Deployment → Service → Ingress) lässt sich direkt anwenden:

```bash
kubectl apply -f snippets/manifests/
```

- [Workloads](workloads.md) – ConfigMap, Deployment
- [Networking](networking.md) – Service, Ingress
- [Storage](storage.md) – PersistentVolume, PersistentVolumeClaim
