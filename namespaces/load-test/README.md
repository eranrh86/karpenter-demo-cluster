# load-test

Load test workers for VPA + DatadogPodAutoscaler demo. Demonstrates in-place vertical scaling (K8s 1.35).

## Manifests
- `configmaps.yaml`
- `deployments.yaml`

## Restore
```bash
kubectl create namespace load-test --dry-run=client -o yaml | kubectl apply -f -
kubectl apply -f namespaces/load-test/
```
