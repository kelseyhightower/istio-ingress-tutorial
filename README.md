# Istio Ingress Tutorial

This tutorial demonstrates how to run the Istio Ingress Controller in a Kubernetes Cluster.

## Rationale

The Istio project hosts multiple components including: [Pilot](https://istio.io/docs/concepts/traffic-management/pilot.html), [Mixer](https://istio.io/docs/concepts/policy-and-control/mixer.html), and [Auth](https://istio.io/docs/concepts/network-and-auth/auth.html). When combined these components provide a complete platform to connect, manage, and secure microservices. However, adopting Istio is not an all or nothing proposition. You can adopt only the parts you need. In this tutorial the Istio Pilot, which is responsible for the lifecycle of [Envoy](https://lyft.github.io/envoy/) instances, and the Istio Ingress, a [Kubernetes Ingress Controller](https://kubernetes.io/docs/concepts/services-networking/ingress/) based on Envoy, will be used to provide a robust Ingress solution.

## Tutorial

* [Prerequisites](docs/01-prerequisites.md)
* [Install Client Tools](docs/02-client-tools.md)
* [Provision The Kubernetes Infrastructure](docs/03-kubernetes-infrastructure.md)
* [Provision The Istio Pilot](docs/04-istio-pilot.md)
* [Provision The Istio Ingress Controller](docs/05-istio-ingress-controller.md)
* [Using Istio Route Rules](docs/06-istio-route-rules.md)

## Clean Up

Run the clean-up bash script to remove all compute resources created by this tutorial:

```
bash clean-up
```
