# monitoring

Full monitoring stack: Prometheus, Grafana, demo app (api/frontend/database), alertmanager, Datadog webhook.

## Manifests
- `configmaps.yaml`
- `daemonsets.yaml`
- `deployments.yaml`
- `services.yaml`
- `statefulsets.yaml`

## Restore
```bash
kubectl create namespace monitoring --dry-run=client -o yaml | kubectl apply -f -
kubectl apply -f namespaces/monitoring/
```
