---
layout: post
title: How to Install Istio on K8S with Helm?
author: Kevser Sirca
categories: [istio]
tags: [istio, kubectl]
---

Istio is an open source service mesh platform that provides a way to control how microservices share data with one another.

## Download Istio

```
curl -L https://istio.io/downloadIstio | ISTIO_VERSION=1.6.8 TARGET_ARCH=x86_64 sh -
```

and cd directory

```
cd istio-1.11.3
```

## Install Istio with Helm

Create istio-system namespace in k8s

```
kubectl create namespace istio-system
```
Install the Istio base chart which contains cluster-wide resources used by the Istio control plane:

```
  helm install istio-base manifests/charts/base -n istio-system
```
Install the Istio discovery chart which deploys the istiod service:

```
helm install istiod manifests/charts/istio-control/istio-discovery -n istio-system
```
Install the Istio ingress gateway chart which contains the ingress gateway components:
```
helm install istio-ingress manifests/charts/gateways/istio-ingress -n istio-system
```

## Verifying the installation

Ensure all Kubernetes pods in istio-system namespace are deployed and have a STATUS of Running:

```
  kubectl get pods -n istio-system
```
If occurs error, please check this doc [Install Istio with Helm](https://istio.io/latest/docs/setup/install/helm/)

Happy Friday,

