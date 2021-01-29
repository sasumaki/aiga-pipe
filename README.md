install fluxctl

add cluster/infrastructure-sync.yaml
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