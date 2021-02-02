Prerequisites:

  * [flux](https://toolkit.fluxcd.io/)

  * [kubeseal](https://toolkit.fluxcd.io/guides/sealed-secrets/)

  * a kubernetes cluster, e.g. [k3d](https://github.com/rancher/k3d)
  
     `$ k3d cluster create test -p 8081:80@loadbalancer --agents 2 --k3s-server-arg '--no-deploy=traefik' --k3s-server-arg '--kubelet-arg=eviction-hard=imagefs.available<1%,nodefs.available<1%' --k3s-server-arg '--kubelet-arg=eviction-minimum-reclaim=imagefs.available=1%,nodefs.available=1%'`

See `github.com/sasumaki/asd` for on how to use



Copypasting these manifests will install the stuff
```
---
apiVersion: source.toolkit.fluxcd.io/v1beta1
kind: GitRepository
metadata:
  name: cluster-system
  namespace: flux
spec:
  interval: 30m0s
  ref:
    branch: main
  url: https://github.com/sasumaki/aiga-pipe
---
apiVersion: kustomize.toolkit.fluxcd.io/v1beta1
kind: Kustomization
metadata:
  name: cluster-system
  namespace: flux
spec:
  interval: 30m0s
  path: ./cluster
  prune: true
  sourceRef:
    kind: GitRepository
    name: cluster-system
  validation: client
```