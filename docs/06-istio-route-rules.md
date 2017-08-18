# Istio Route Rules

The Istio Ingress Controller provides an [advanced feature set](https://istio.io/docs/concepts/traffic-management/rules-configuration.html) based Envoy's unique traffic management capabilities. This section will demonstrates how to dynamically configure the Istio Ingress Controller using the Istio Pilot and the `istioclt` command line tool.

The `istio-test` HTTP service will be used to demonstrate Istio's traffic management features.

## Test the Istio Ingress Controller

Deploy `v1` of the `istio-test` application:

```
kubectl apply -f deployments/istio-test-v1.yaml
```

Expose the `istio-test-v1` deployment:

```
kubectl apply -f services/istio-test.yaml
```

Create the `istio-test` ingress object:

```
kubectl apply -f ingress/istio-test.yaml
```

### Interact with the Istio Ingress Controller

In this section the `curl` command line tool will be used to test the basic functionality of the Istio Ingress Controller.

Retrieve the `istio-ingress` external IP address:

```
ISTIO_INGRESS_IP=$(kubectl get svc istio-ingress \
  -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
```

```
curl -i http://${ISTIO_INGRESS_IP}
```

```
HTTP/1.1 200 OK
date: Fri, 18 Aug 2017 16:26:40 GMT
content-length: 14
content-type: text/plain; charset=utf-8
x-envoy-upstream-service-time: 1
server: envoy

istio-test-v1
```

## Testing Istio Route rules

The Isito Ingress Controller can be dynamically configured by the Istio Pilot to provide more advanced routing between external clients and services running in a Kubernetes cluster. To demonstrate these features we need to deploy `v2` of the `istio-test` HTTP service:

```
kubectl apply -f deployments/istio-test-v2.yaml
```

At this point `v1` and `v2` of the `istio-test` service should be running:

```
kubectl get pods -l app=istio-test
```
```
NAME                             READY     STATUS    RESTARTS   AGE
istio-test-v1-XXXXXXXXX-XXXXX    1/1       Running   0          5m
istio-test-v2-XXXXXXXXX-XXXXX    1/1       Running   0          4m
```

Run the `curl` command a few times and observe the response from the Istio Ingress Controller:

```
curl http://${ISTIO_INGRESS_IP}
```

```
istio-test-v1
```

```
curl http://${ISTIO_INGRESS_IP}
```

```
istio-test-v2
```

Notice both `v1` and `v2` of the `istio-test` service are responding.

### Add a Route Rule

An [Istio Route Rule](https://istio.io/docs/concepts/traffic-management/rules-configuration.html) can be used to force all clients to hit `v2` of the `istio-test` service.

Create a route rule and save it to a file named `istio-test-default.yaml`:

```
cat > istio-test-default.yaml <<EOF
type: route-rule
name: istio-test-default
namespace: default
spec:
  destination: istio-test.default.svc.cluster.local
  route:
    - tags:
        version: v2
      weight: 100
EOF
```

Submit the route rule using the `istioctl` command:

```
istioctl create -f istio-test-default.yaml
```

Route rules are stored in Kubernetes [ThirdPartyResources](https://kubernetes.io/docs/tasks/access-kubernetes-api/extend-api-third-party-resource/)

```
kubectl get thirdpartyresources
```
```
NAME                    DESCRIPTION           VERSION(S)
istio-config.istio.io   Istio configuration   v1alpha1
```

Inspect the details of the `istio-test-default` route rule configuration:

```
kubectl get istioconfigs
```
```
NAME                            KIND
route-rule-istio-test-default   IstioConfig.v1alpha1.istio.io
```

Use the `kubectl describe` command to get more details:

```
kubectl describe istioconfigs route-rule-istio-test-default
```
```
Name:         route-rule-istio-test-default
Namespace:    default
Labels:       <none>
Annotations:  <none>
API Version:  istio.io/v1alpha1
Kind:         IstioConfig
Metadata:
  Creation Timestamp:   2017-08-18T16:29:34Z
  Resource Version:     10775
  Self Link:            /apis/istio.io/v1alpha1/namespaces/default/istioconfigs/route-rule-istio-test-default
  UID:                  6b7530f4-8432-11e7-a05d-42010a80004f
Spec:
  Destination:  istio-test.default.svc.cluster.local
  Route:
    Tags:
      Version:  v2
    Weight:     100
Events:         <none>
```

The `istioctl` command can also be used:

```
istioctl get route-rules istio-test-default
```
```
destination: istio-test.default.svc.cluster.local
route:
- tags:
    version: v2
  weight: 100
```

### Interact with the Istio Ingress Controller

With the `istio-test-default` route rule in place re-run the `curl` command a few times and observe the response:

```
curl http://${ISTIO_INGRESS_IP}
```

```
istio-test-v2
```

```
curl http://${ISTIO_INGRESS_IP}
```

```
istio-test-v2
```

Notice all traffic is handeled by `v2` of the `istio-test` service.
