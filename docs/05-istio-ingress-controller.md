# Istio Ingress Controller

## Provision the Istio Ingress Controller

### Create the Istio Ingress Service Account and Cluster Role

Create the `istio-ingress` service account:

```
kubectl apply -f serviceaccounts/istio-ingress.yaml
```

Create the `istio-ingress` cluster role:

```
kubectl apply -f clusterroles/istio-ingress.yaml
```

Create the `istio-ingress` role binding:

```
kubectl apply -f rolebindings/istio-ingress.yaml
```

### Create the Istio Ingress DaemonSet

A DaemonSet will be used to ensure a single Istio Ingress Controller runs on each node in the `istio-ingress-pool` node pool.

```
kubectl apply -f daemonsets/istio-ingress.yaml
```

### Expose the Istio Ingress Controllers

Create the `istio-ingress` service:

```
kubectl apply -f services/istio-ingress.yaml
```

> The `istio-ingress` service sets the `spec.externalTrafficPolicy` to `Local` to ensure only the nodes in the `istio-ingress-pool` node pool running the Istio Ingress Controller receive traffic. This feature is documented in the [using source IP services section](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer)

At this point the Istio Ingress Controllers are ready to receive traffic:

```
kubectl get pods -l istio=ingress
```

```
NAME                  READY     STATUS    RESTARTS   AGE
istio-ingress-XXXXX   1/1       Running   0          5m
istio-ingress-XXXXX   1/1       Running   0          5m
```

Next: [Using Istio Route Rules](06-istio-route-rules.md)
