---
layout: post
title: How to Setup Knative on K8S?
author: Kevser Sirca
categories: [knative]
tags: [knative, kubectl]
---

If you want to install Knative on production,

* You have a cluster that uses Kubernetes v1.19 or newer.
* You have installed the kubectl CLI.
* Your Kubernetes cluster must have access to the internet.

## Install the Knative Serving component

Install the required custom resources by running the command:
```
  kubectl apply -f https://github.com/knative/serving/releases/download/v0.26.0/serving-crds.yaml
```

Install the core components of Knative Serving by running the command:
```
  kubectl apply -f https://github.com/knative/serving/releases/download/v0.26.0/serving-core.yaml
```

## Install a networking layer

I prefer to istio. If you haven't istio please first install it.

* 1- install istio on k8s
* 2- install istio for knative

and then
