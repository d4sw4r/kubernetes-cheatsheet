---
tags: [kubectl]
---
# kubectl

## Get

```bash
kubectl get services                          # List all services in the namespace
kubectl get pods --all-namespaces             # List all pods in all namespaces
kubectl get pods -o wide                      # List all pods in the current namespace, with more details
kubectl get deployment my-dep                 # List a particular deployment
kubectl get pods                              # List all pods in the namespace
kubectl get pod my-pod -o yaml                # Get a pod's YAML
```

## Describe

```bash
kubectl describe nodes my-node
kubectl describe pods my-pod
```

## Create & apply

```bash
kubectl apply -f ./my-manifest.yaml            # create resource(s)
kubectl apply -f ./my1.yaml -f ./my2.yaml      # create from multiple files
kubectl apply -f ./dir                         # create resource(s) in all manifest files in dir
kubectl apply -f https://git.io/vPieo          # create resource(s) from url
kubectl create deployment nginx --image=nginx  # start a single instance of nginx
```

## Scaling

```bash
kubectl scale --replicas=3 rs/foo                                 # Scale a replicaset named 'foo' to 3
kubectl scale --replicas=3 -f foo.yaml                            # Scale a resource specified in "foo.yaml" to 3
kubectl scale --current-replicas=2 --replicas=3 deployment/mysql  # If the deployment named mysql's current size is 2, scale mysql to 3
kubectl scale --replicas=5 rc/foo rc/bar rc/baz                   # Scale multiple replication controllers
```

## Deleting

```bash
kubectl delete -f ./pod.json                                      # Delete a pod using the type and name specified in pod.json
kubectl delete pod unwanted --now                                 # Delete a pod with no grace period
kubectl delete pod,service baz foo                                # Delete pods and services with same names "baz" and "foo"
kubectl delete pods,services -l name=myLabel                      # Delete pods and services with label name=myLabel
kubectl -n my-ns delete pod,svc --all                             # Delete all pods and services in namespace my-ns
```

## Logging

```bash
kubectl logs my-pod                                 # dump pod logs (stdout)
kubectl logs -l name=myLabel                        # dump pod logs, with label name=myLabel (stdout)
kubectl logs my-pod --previous                      # dump pod logs (stdout) for a previous instantiation of a container
kubectl logs my-pod -c my-container                 # dump pod container logs (stdout, multi-container case)
kubectl logs -l name=myLabel -c my-container        # dump pod logs, with label name=myLabel (stdout)
kubectl logs my-pod -c my-container --previous      # dump pod container logs (stdout, multi-container case) for a previous instantiation of a container
kubectl logs -f my-pod                              # stream pod logs (stdout)
```

## Taints

```bash
kubectl taint node <NAME> node-role.kubernetes.io/control-plane:NoSchedule-   # Control-Plane-Taint entfernen
kubectl taint node <NAME> node-role.kubernetes.io/master:NoSchedule-          # dito, ältere Cluster
```

## Nodes & Cluster

```bash
kubectl cordon my-node                                                # Mark my-node as unschedulable
kubectl drain my-node                                                 # Drain my-node in preparation for maintenance
kubectl uncordon my-node                                              # Mark my-node as schedulable
kubectl top node my-node                                              # Show metrics for a given node
kubectl cluster-info                                                  # Display addresses of the master and services
kubectl cluster-info dump                                             # Dump current cluster state to stdout
kubectl cluster-info dump --output-directory=/path/to/cluster-state   # Dump current cluster state to /path/to/cluster-state
```

## Rollout

```bash
kubectl rollout restart deployment/my-dep            # Pods neu starten (z. B. nach ConfigMap-Änderung)
kubectl rollout status deployment/my-dep             # Warten bis fertig
kubectl rollout undo deployment/my-dep               # Letzte Revision zurück
```

## Secrets

```bash
kubectl get secret my-secret -n my-ns --template={{.data.password}} | base64 -d     # Einen Key auslesen
kubectl create secret docker-registry regcred --docker-server=<registry> \
  --docker-username=<user> --docker-password=<pass>                                  # Pull-Secret
```

## Löschen mit Wildcard

`kubectl delete` kennt keine Wildcards – Namen per `awk` filtern und an `xargs` geben:

```bash
kubectl get configmap -n my-ns --no-headers=true | awk '/dashboard-configmap/{print $1}'   # erst anschauen!
kubectl get configmap -n my-ns --no-headers=true | awk '/dashboard-configmap/{print $1}' | xargs kubectl delete -n my-ns configmap
```

## Lokales Image in minikube

```bash
eval $(minikube docker-env)                     # Docker-CLI auf den minikube-Daemon zeigen lassen
docker build -t in-cluster .
kubectl run --rm -i demo --image=in-cluster --image-pull-policy=Never
```

## Quellen

- [kubectl Quick Reference](https://kubernetes.io/docs/reference/kubectl/quick-reference/)
