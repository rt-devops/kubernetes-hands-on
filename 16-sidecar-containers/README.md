# Sidecar containers: what, why, and how

## Introduction

In kubernetes a pod can contain multiple containers:

```yml
apiVersion: v1
kind: Pod
metadata:
  name: simple-pod
spec:
  containers:
  - name: container1
    image: nginx
  - name: container2
    image: fluentd
```

Here we have 2 containers: `container1` and `container2`.

When you have multiple containers in a pod we call them sidecar containers. Most of the time you have a "primary" container, the one with containing your application, and "secondary", hence the sidecar terminology.

All the containers of a given pod share the same network, and can share the same volumes.

## Use cases

Sidecars are useful for containers that are tightly coupled. A good use case is when you migrate an app from one machine to containers. The application will have a lot of `localhost` or `127.0.0.1` hardcoded. So with sidecars you can work around this.

Another use case is to have sidecars helping the main container, like sending logs to a centralized system, sending the metrics to a specific system, doing SSL termination, etc.

Istio, the service mesh tool, installs a sidecar container to do its job: https://istio.io/docs/setup/kubernetes/additional-setup/sidecar-injection/

## Exercices

Review and apply the file [01-sidecar.yml](01-sidecar.yml).

Connect to the `nginx` container (`kubectl exec -it two-containers -c nginx -- /bin/sh`) and look at the file system in `/var/log/nginx`.

Do the same for the `fluentd` container and this time look in `/logs`. What do you see?

Tail the logs from the fluentd pod (`kubectl logs -f two-containers -c fluentd`) and in another terminal window, send requests to the nginx service with a `curl` from a separate pod (or nginx's pod itself). What do you see? How do you explain it?

This exercice is taken from the [official Kubernetes documentation](https://kubernetes.io/docs/tasks/access-application-cluster/communicate-containers-same-pod-shared-volume/#creating-a-pod-that-runs-two-containers).

## Clean up

```sh
kubectl delete service,deployment,pod,configmap --all
```

## Links

* https://docs.microsoft.com/en-us/azure/architecture/patterns/sidecar
* https://medium.com/@dwdraju/sidecar-pattern-with-use-case-examples-ed6642e5eaf7
