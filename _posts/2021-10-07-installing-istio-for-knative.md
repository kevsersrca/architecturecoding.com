---
layout: post
title: Installing Istio for Knative
author: Kevser Sirca
categories: [knative]
tags: [istio, knative, kubectl]
---

## Installing Istio with sidecar injection
If you want to enable the Istio service mesh, you must enable automatic sidecar injection. The Istio service mesh provides a few benefits:

* Allows you to turn on mutual TLS, which secures service-to-service traffic within the cluster.
* Allows you to use the Istio authorization policy, controlling the access to each Knative service based on Istio service roles.

##### Enable sidecar container on knative-serving system namespace
```
  kubectl label namespace knative-serving istio-injection=enabled
```

##### Set PeerAuthentication to PERMISSIVE on knative-serving system namespace by creating a YAML file using the following template:

```
apiVersion: "security.istio.io/v1beta1"
kind: "PeerAuthentication"
metadata:
  name: "default"
  namespace: "knative-serving"
spec:
  mtls:
    mode: PERMISSIVE
```

##### Apply the YAML file by running the command:
```
  kubectl apply -f filename.yaml
```
##### Updating the config-istio configmap to use a non-default local gateway
If you create a custom service and deployment for local gateway with a name other than `knative-local-gateway`, you need to update gateway configmap `config-istio` under the `knative-serving` namespace.

1- Edit the config-istio configmap:
```
  kubectl edit configmap config-istio -n knative-serving
```


If occurs error, following link [Knative DOC](https://knative.dev/docs/admin/install/installing-istio/#installing-istio-with-sidecar-injection)