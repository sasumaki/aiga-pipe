Prerequisites:
  flux
  kubeseal
  a kubernetes cluster, e.g. `k3d cluster create test -p 8081:80@loadbalancer --agents 2 --k3s-server-arg '--no-deploy=traefik' --k3s-server-arg '--kubelet-arg=eviction-hard=imagefs.available<1%,nodefs.available<1%' --k3s-server-arg '--kubelet-arg=eviction-minimum-reclaim=imagefs.available=1%,nodefs.available=1%'`

See `github.com/sasumaki/asd` for on how to use


```
---
apiVersion: source.toolkit.fluxcd.io/v1beta1
kind: GitRepository
metadata:
  name: cluster-system
  namespace: flux
spec:
  interval: 2m0s
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
  interval: 10m0s
  path: ./cluster
  prune: true
  sourceRef:
    kind: GitRepository
    name: cluster-system
  validation: client
```