# datadog

Datadog Agent DaemonSet + Cluster Agent (secondary install). Separate from the Datadog Operator managed agents in `default`.

## Manifests
- `configmaps.yaml`
- `daemonsets.yaml`
- `deployments.yaml`
- `rbac.yaml`
- `serviceaccounts.yaml`
- `services.yaml`

## Restore
```bash
kubectl create namespace datadog --dry-run=client -o yaml | kubectl apply -f -
kubectl apply -f namespaces/datadog/
```
