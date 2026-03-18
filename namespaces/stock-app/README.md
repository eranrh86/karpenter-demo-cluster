# stock-app

Stock trading demo app. Flask backend (real-time prices), nginx frontend (Plus500-like UI), PostgreSQL.

## Manifests
- `configmaps.yaml`
- `deployments.yaml`
- `rbac.yaml`
- `services.yaml`
- `statefulsets.yaml`

## Restore
```bash
kubectl create namespace stock-app --dry-run=client -o yaml | kubectl apply -f -
kubectl apply -f namespaces/stock-app/
```
