---
layout: post
title: Kubernetes Cheat sheet For Beginner 
author: Kevser Sirca
categories: [kubernetes]
tags: [kubernetes, k8s, kubectl]
---
Kubernetes cheat sheet is for those who want to start kubernetes and don't know how to start or to remember forgotten commands.

## Table of Contents

<div class="table-of-contents"><ul><li><a href="#kubectl-alias">Kubectl Alias</a></li><li><a href="#cluster-info">Cluster Info</a></li><li><a href="#contexts">Contexts</a></li><li><a href="#get-commands">Get Commands</a></li><li><a href="#namespaces">Namespaces</a></li><li><a href="#labels">Labels</a></li><li><a href="#describe-command">Describe Command</a></li><li><a href="#delete-command">Delete Command</a></li><li><a href="#create-vs-apply">Create vs Apply</a></li><li><a href="#create-pod">Create Pod</a></li><li><a href="#create-deployment">Create Deployment</a></li><li><a href="#create-service">Create Service</a></li><li><a href="#export-yaml-for-new-pod">Export YAML for New Pod</a></li><li><a href="#export-yaml-for-existing-object">Export YAML for Existing Object</a></li><li><a href="#logs">Logs</a></li><li><a href="#port-forward">Port Forward</a></li><li><a href="#scaling">Scaling</a></li><li><a href="#autoscaling">Autoscaling</a></li><li><a href="#rollout">Rollout</a></li><li><a href="#pod-example">Pod Example</a></li><li><a href="#deployment-example">Deployment Example</a></li><li><a href="#dashboard">Dashboard</a></li></ul></div>

## Definition

A cheat sheet for Kubernetes commands.

`Master`: The machine that controls Kubernetes nodes. This is where all task assignments originate.

`Node`: These machines perform the requested, assigned tasks. The Kubernetes master controls them.

`Pod`: A group of one or more containers deployed to a single node. All containers in a pod share an IP address, IPC, hostname, and other resources. Pods abstract network and storage away from the underlying container. This lets you move containers around the cluster more easily.

`Replication controller`:  This controls how many identical copies of a pod should be running somewhere on the cluster.

`Service`: This decouples work definitions from the pods. Kubernetes service proxies automatically get service requests to the right pod—no matter where it moves to in the cluster or even if it’s been replaced.

`Kubelet`: This service runs on nodes, reads the container manifests, and ensures the defined containers are started and running.

`kubectl`: This is the command line configuration tool for Kubernetes.

![kubernetes](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRcuCYNMCaMQHXK5HZMzso-t8H8SpZhoVG_1g&usqp=CAU "Kubernetes Diagram")

## Kubectl Alias

Linux and OSX
{% highlight js %}
$ alias k=kubectl
{% endhighlight%}

Windows
{% highlight js %}
$ Set-Alias -Name k -Value kubectl
{% endhighlight%}

## Cluster Info

- Get clusters
{% highlight js %}
$ kubectl config get-clusters
{% endhighlight%}

- Get cluster info.
{% highlight js %}
$ kubectl cluster-info
Kubernetes master is running at https://172.17.0.58:8443
{% endhighlight%}

## Contexts

A context is a cluster, namespace and user.

- Get a list of contexts.

{% highlight js %}
$ kubectl config get-contexts
{% endhighlight%}

{% highlight js %}
CURRENT   NAME                 CLUSTER                      AUTHINFO             NAMESPACE
          docker-desktop       docker-desktop               docker-desktop
*         foo                  foo                          foo                  bar
{% endhighlight%}

- Get the current context.
{% highlight js %}
$ kubectl config current-context
foo
{% endhighlight%}

- Switch current context.
{% highlight js %}
$ kubectl config use-conext docker-desktop
{% endhighlight%}

- Set default namesapce
```
$ kubectl config set-context $(kubectl config current-context) --namespace=my-namespace
```

To switch between contexts, you can also install and use [kubectx](https://github.com/ahmetb/kubectx).

## Get Commands

```
$ kubectl get all
$ kubectl get namespaces
$ kubectl get services
$ kubectl get replicationcontroller
$ kubectl get deployments
$ kubectl get ingress
$ kubectl get configmaps
$ kubectl get nodes
$ kubectl get pods
$ kubectl get rs
$ kubectl get svc kuard
$ kubectl get endpoints kuard
```

Additional switches that can be added to the above commands:

- `-o wide` - show more information.
- `--watch` or `-w` - watch for changes.

## Namespaces

- `--namespace` - Get a resource for a specific namespace.

You can set the default namespace for the current context like so:

```
$ kubectl config set-context $(kubectl config current-context) --namespace=my-namespace
```

To switch namespaces, you can also install and use [kubens](https://github.com/ahmetb/kubectx/blob/master/kubens).

## Labels

- Get pods showing labels.
```
$ kubectl get pods --show-labels
```

- Get pods by label.
```
$ kubectl get pods -l environment=production,tier!=frontend
$ kubectl get pods -l 'environment in (production,test),tier notin (frontend,backend)'
```

## Describe Command

```
$ kubectl describe nodes [id]
$ kubectl describe pods [id]
$ kubectl describe rs [id]
$ kubectl describe svc kuard [id]
$ kubectl describe endpoints kuard [id]
```

## Delete Command

```
$ kubectl delete nodes [id]
$ kubectl delete pods [id]
$ kubectl delete rs [id]
$ kubectl delete svc kuard [id]
$ kubectl delete endpoints kuard [id]
```

Force a deletion of a pod without waiting for it to gracefully shut down
```
$ kubectl delete pod-name --grace-period=0 --force
```

## Create vs Apply

`kubectl create` can be used to create new resources while `kubectl apply` inserts or updates resources while maintaining any manual changes made like scaling pods.

- `--record` - Add the current command as an annotation to the resource.
- `--recursive` - Recursively look for yaml in the specified directory.

## Create Pod

```
$ kubectl run kuard --generator=run-pod/v1 --image=gcr.io/kuar-demo/kuard-amd64:1 --output yaml --export --dry-run > kuard-pod.yml
$ kubectl apply -f kuard-pod.yml
```

## Create Deployment

```
$ kubectl run kuard --image=gcr.io/kuar-demo/kuard-amd64:1 --output yaml --export --dry-run > kuard-deployment.yml
$ kubectl apply -f kuard-deployment.yml
```

## Create Service

```
$ kubectl expose deployment kuard --port 8080 --target-port=8080 --output yaml --export --dry-run > kuard-service.yml
$ kubectl apply -f kuard-service.yml
```

## Export YAML for New Pod

```
$ kubectl run my-cool-app —-image=me/my-cool-app:v1 --output yaml --export --dry-run > my-cool-app.yaml
```

## Export YAML for Existing Object

```
$ kubectl get deployment my-cool-app --output yaml --export > my-cool-app.yaml
```

## Logs

- Get logs.
```
$ kubectl logs -l app=kuard
```

- Get logs for previously terminated container.
```
$ kubectl logs POD_NAME --previous
```

- Watch logs in real time.
```
$ kubectl attach POD_NAME
```

- Copy files out of pod (Requires `tar` binary in container).
```
$ kubectl cp POD_NAME:/var/log .
```

You can also install and use [kail](https://github.com/boz/kail).

## Port Forward

```
$ kubectl port-forward deployment/kuard 8080:8080
```

## Scaling

- Update replicas.
```
$ kubectl scale deployment nginx-deployment --replicas=10
```

## Autoscaling

- Set autoscaling config.
```
vkubectl autoscale deployment nginx-deployment --min=10 --max=15 --cpu-percent=80
```

## Rollout

- Get rollout status.
```
$ kubectl rollout status deployment/nginx-deployment
Waiting for rollout to finish: 2 out of 3 new replicas have been updated...
deployment "nginx-deployment" successfully rolled out
```

- Get rollout history.
```
$ kubectl rollout history deployment/nginx-deployment
$ kubectl rollout history deployment/nginx-deployment --revision=2
```

- Undo a rollout.
```
$ kubectl rollout undo deployment/nginx-deployment
$ kubectl rollout undo deployment/nginx-deployment --to-revision=2
```

- Pause/resume a rollout
```
$ kubectl rollout pause deployment/nginx-deployment
$ kubectl rollout resume deploy/nginx-deployment
```

## Pod Example

```
apiVersion: v1
kind: Pod
metadata:
  name: cuda-test
spec:
  containers:
    - name: cuda-test
      image: "k8s.gcr.io/cuda-vector-add:v0.1"
      resources:
        limits:
          nvidia.com/gpu: 1
  nodeSelector:
    accelerator: nvidia-tesla-p100
 ```

## Deployment Example

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: my-namespace
  labels:
    - environment: production,
    - teir: frontend
  annotations:
    - key1: value1,
    - key2: value2
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```

## Dashboard

- Enable proxy

```
$ kubectl proxy
```

if you want to improve please pull request at [repo](https://github.com/kevsersrca/architecturecoding.com/pulls)