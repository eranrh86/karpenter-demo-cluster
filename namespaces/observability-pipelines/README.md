# observability-pipelines

Observability Pipelines Worker (Vector 0.43). Filters stock-app logs and routes to Datadog Flex Logs index.

## Manifests
- `configmaps.yaml`
- `deployments.yaml`
- `hpa.yaml`
- `services.yaml`

## Restore
```bash
kubectl create namespace observability-pipelines --dry-run=client -o yaml | kubectl apply -f -
kubectl apply -f namespaces/observability-pipelines/
```
