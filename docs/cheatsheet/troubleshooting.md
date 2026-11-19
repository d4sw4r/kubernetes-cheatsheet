---
tags: [kubectl, debugging]
---
# Troubleshooting

## Pod startet nicht

```bash
kubectl get pods -o wide                         # Status, Node, Restarts
kubectl describe pod my-pod                      # Events am Ende lesen!
kubectl get events --sort-by=.lastTimestamp -A   # Cluster-weite Events
kubectl logs my-pod --previous                   # Logs des abgestürzten Containers
```

| Status | Typische Ursache |
|---|---|
| `Pending` | Keine Ressourcen, kein passender Node (Taints/Affinity), PVC nicht gebunden |
| `ImagePullBackOff` | Falscher Image-Name/Tag, fehlendes `imagePullSecret` |
| `CrashLoopBackOff` | App crasht – `logs --previous` prüfen |
| `OOMKilled` | Memory-Limit zu niedrig |

## In den Cluster schauen

```bash
kubectl exec -it my-pod -- sh                                  # Shell im Container
kubectl debug -it my-pod --image=busybox --target=my-container # Ephemeral Debug-Container
kubectl run tmp --rm -it --image=nicolaka/netshoot -- bash     # Netzwerk-Debugging
kubectl port-forward svc/nginx-service 8080:80                 # Service lokal erreichbar machen
```

## Gotchas / Learnings

!!! note "TODO"
    Eigene Fälle ergänzen.
