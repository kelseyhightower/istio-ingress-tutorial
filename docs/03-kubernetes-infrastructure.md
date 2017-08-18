# Kubernetes Infrastructure

## Provision A Kubernetes Cluster

A Kubernetes 1.7.3+ cluster is required to host the Istio Pilot and Ingress Controller. Use the gcloud command to provision a two node Kubernetes cluster:

```
gcloud container clusters create istio \
  --machine-type n1-standard-1 \
  --num-nodes 2 \
  --cluster-version 1.7.3
```

It can take several minutes to provision the `istio` Kubernetes cluster. Either wait for the above command to complete or use the gcloud command to monitor progress in a separate terminal:

```
gcloud container clusters list
```

```
NAME   ZONE           MASTER_VERSION  MASTER_IP      MACHINE_TYPE   NODE_VERSION  NUM_NODES  STATUS
istio  us-central1-f  1.7.3           XX.XXX.XXX.XX  n1-standard-1  1.7.3         2          PROVISIONING
```

> Estimated time to completion: 5 minutes.

### Provision an Istio Ingress Node Pool

Add an additional node pool to support running multiple Istio Ingress controllers across a dedicated set of machines.

```
gcloud container node-pools create istio-ingress-pool \
  --cluster istio \
  --machine-type n1-standard-1 \
  --num-nodes 2 \
  --node-labels dedicated=istio-ingress
```

> Estimated time to completion: 2 minutes.

List the node pools for the `istio` Kubernetes cluster:

```
gcloud container node-pools list --cluster istio
```

```
NAME                MACHINE_TYPE   DISK_SIZE_GB  NODE_VERSION
default-pool        n1-standard-1  100           1.7.3
istio-ingress-pool  n1-standard-1  100           1.7.3
```

It can take several minutes before the `istio` Kubernetes cluster is ready. Use the gcloud command to monitor progress:

```
gcloud container clusters list
```

```
NAME   ZONE           MASTER_VERSION  MASTER_IP      MACHINE_TYPE   NODE_VERSION  NUM_NODES  STATUS
istio  us-central1-f  1.7.3           XXX.XXX.XX.XX  n1-standard-1  1.7.3         4          RECONCILING
```

> Estimated time to completion: 3 minutes.

### Taint the Istio Ingress Node Pool

Ensure nodes in the `istio-ingress-pool` node pool only accept Istio Ingress workloads by tainting them:

```
kubectl taint nodes \
  $(kubectl get nodes -l dedicated=istio-ingress -o jsonpath='{.items[*].metadata.name}') \
  dedicated=istio-ingress:NoSchedule
```

```
node "gke-istio-istio-ingress-pool-XXXXXXXX-XXXX" tainted
node "gke-istio-istio-ingress-pool-XXXXXXXX-XXXX" tainted
```

### Grant Cluster Admin Permissions to the current User

Admin permissions are required to create the necessary RBAC rules for the Istio Pilot and Ingress Controller:

```
kubectl create clusterrolebinding cluster-admin-binding \
  --clusterrole=cluster-admin \
  --user=$(gcloud config get-value core/account)
```

Next: [Provision The Istio Pilot](04-istio-pilot.md)
