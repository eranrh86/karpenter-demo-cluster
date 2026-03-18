# yes001

Datadog RUM demo for yes.co.il TV. Shows beforeSend filtering reducing events by ~83%.

## Manifests
- `configmaps.yaml`
- `deployments.yaml`
- `services.yaml`

## Restore
```bash
kubectl create namespace yes001 --dry-run=client -o yaml | kubectl apply -f -
kubectl apply -f namespaces/yes001/
```
