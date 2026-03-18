# cloudprem

Datadog CloudPrem BYOC log management. Logs stored in S3, indexed by Quickwit. Deployed via Helm chart `datadog/cloudprem`.

## Manifests
- `configmaps.yaml`
- `deployments.yaml`
- `rbac.yaml`
- `serviceaccounts.yaml`
- `services.yaml`
- `statefulsets.yaml`

## Restore
```bash
kubectl create namespace cloudprem --dry-run=client -o yaml | kubectl apply -f -
kubectl apply -f namespaces/cloudprem/
```
