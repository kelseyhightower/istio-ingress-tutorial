# Istio Pilot

## Provision the Istio Pilot

### Create the Istio Pilot Service Account and Cluster Role

Create the `istio-pilot` service account:

```
kubectl apply -f serviceaccounts/istio-pilot.yaml
```

Create the `istio-pilot` cluster role:

```
kubectl apply -f clusterroles/istio-pilot.yaml
```

Create the `istio-pilot` role binding:

```
kubectl apply -f rolebindings/istio-pilot.yaml
```

### Create the Istio ConfigMap

Create the `istio` ConfigMap which holds the Istio configuration:

```
kubectl apply -f configmaps/istio.yaml
```

### Create the Istio Pilot Deployment

Create the `istio-pilot` deployment:

```
kubectl apply -f deployments/istio-pilot.yaml
```

### Expose the Istio Pilot Deployment

Create the `istio-pilot` service:

```
kubectl apply -f services/istio-pilot.yaml
```

At this point the Istio Pilot service is ready for use:

```
kubectl get pods -l istio=pilot
```

```
NAME                          READY     STATUS    RESTARTS   AGE
istio-pilot-XXXXXXXXX-XXXXX   2/2       Running   0          1m
```

Next: [Provision The Istio Ingress Controller](05-istio-ingress-controller.md)
