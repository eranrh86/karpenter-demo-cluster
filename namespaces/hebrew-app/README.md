# hebrew-app

Hebrew language demo application with Flask backend, nginx frontend, and Redis cache.

## Manifests
- `configmaps.yaml`
- `deployments.yaml`
- `services.yaml`

## Restore
```bash
kubectl create namespace hebrew-app --dry-run=client -o yaml | kubectl apply -f -
kubectl apply -f namespaces/hebrew-app/
```
