This repository is an attempt to lock down [weaveowrks/flux](https://github.com/weaveworks/flux/) as much as possible
without error messages from Flux.

[flux-system/](flux-system) contains a Flux deployment that is limited to resources in the `helloworld` namespace.

[helloworld-rbac/](helloworld-rbac) contains the namespace and minimum `Role` and `RoleBinding` necessary to give Flux
access to manage the simple hello world service defined in [helloworld-flux/](helloworld-flux).


## Setup

Deploy Flux to the cluster:

```sh
kubectl apply -f flux-system -f helloworld-rbac
```

This will create two namespaces:

* `flux-system` with deployments for memcached and Flux limited to the other namespace,
* `helloworld`, which contains a `Role` giving Flux permissions required to manage our hello world service


Point `fluxctl` at our Flux instance and print the SSH key:

```sh
export FLUX_FORWARD_NAMESPACE=flux-system
export FLUX_FORWARD_LABELS="app=flux,component=weave-flux"

fluxctl identity
```

Flux should now be able to just manage our hello world service without giving any errors.
