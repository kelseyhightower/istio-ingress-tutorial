# Prerequisites

## Google Cloud Platform

This tutorial leverages the [Google Cloud Platform](https://cloud.google.com/) to streamline the provisioning of a Kubernetes cluster and the necessary compute infrastructure required to run the Istio Pilot and Ingress Controller. [Sign up](https://cloud.google.com/free/) for $300 in free credits.

[Estimated cost](https://cloud.google.com/products/calculator/#id=e2729d5d-4a6d-4f7e-aebf-47e6e87ee5d5) to run this tutorial: $0.16 per hour ($3.79 per day).

> The compute resources required for this tutorial exceed the Google Cloud Platform free tier.

## Source

The Istio Ingress Tutorial repository holds a collection of configuration files and scripts that must be downloaded onto the machine used to follow this tutorial. Use the `git` command to clone this repository:

```
git clone https://github.com/kelseyhightower/istio-ingress-tutorial.git
```

The remainder of this tutorial assumes your working directory is the root of the cloned repository. Move into the `istio-ingress-tutorial` directory:

```
cd istio-ingress-tutorial
```

Next: [Install Client Tools](02-client-tools.md)
