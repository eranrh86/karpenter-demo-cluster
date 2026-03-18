# live-migration

Zero-downtime pod live migration demo using CRIU v3.17.1 + Kube-OVN IP preservation on EKS.

## Manifests
- `configmaps.yaml`
- `daemonsets.yaml`
- `rbac.yaml`
- `services.yaml`

## Restore
```bash
kubectl create namespace live-migration --dry-run=client -o yaml | kubectl apply -f -
kubectl apply -f namespaces/live-migration/
```
