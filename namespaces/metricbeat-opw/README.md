# metricbeat-opw

Metricbeat DaemonSet shipping metrics to local Elasticsearch + Vector (OPW) pipeline.

## Manifests
- `configmaps.yaml`
- `daemonsets.yaml`
- `deployments.yaml`
- `services.yaml`

## Restore
```bash
kubectl create namespace metricbeat-opw --dry-run=client -o yaml | kubectl apply -f -
kubectl apply -f namespaces/metricbeat-opw/
```
