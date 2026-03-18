# githubci

GitHub Actions self-hosted runners via Actions Runner Controller (ARC). Runners registered to repo `eranrh86/Karpenter-CI`.

## Manifests
- `configmaps.yaml`
- `deployments.yaml`
- `rbac.yaml`
- `services.yaml`

## Restore
```bash
kubectl create namespace githubci --dry-run=client -o yaml | kubectl apply -f -
kubectl apply -f namespaces/githubci/
```
