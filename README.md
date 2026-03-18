# karpenter-demo — EKS Cluster Documentation

> **Primary demo cluster** running all Datadog SE projects on Amazon EKS (us-east-1, account `770341584863`)

## Cluster Info
| Property | Value |
|---|---|
| Name | `karpenter-demo` |
| Region | `us-east-1` |
| EKS Version | `1.35` |
| Node OS | Amazon Linux 2023 |
| CNI | Kube-OVN v1.15.4 (overlay, 10.16.0.0/16) |
| Autoscaler | Karpenter v1.8.6 |
| Container Runtime | containerd |

## Projects (Namespaces)

| Namespace | Project | Description |
|---|---|---|
| [`cloudprem`](namespaces/cloudprem/) | Datadog CloudPrem | BYOC log management — logs stored in S3 (`eran-cloudprem-karpenter`), indexed via Quickwit |
| [`datadog`](namespaces/datadog/) | Datadog Agent | DaemonSet + Cluster Agent (separate from default ns agents) |
| [`default`](namespaces/default/) | Main Workloads | eran-frontend/backend/database, load generators, Datadog Operator, karpenter-test, javaapp |
| [`githubci`](namespaces/githubci/) | GitHub Actions | ARC controller + demo runners (repo: eranrh86/Karpenter-CI) |
| [`hebrew-app`](namespaces/hebrew-app/) | Hebrew App | Demo app with frontend, backend, redis |
| [`live-migration`](namespaces/live-migration/) | CRIU Live Migration | Zero-downtime pod migration using CRIU + Kube-OVN |
| [`load-test`](namespaces/load-test/) | Load Test | VPA + DatadogPodAutoscaler demo workloads |
| [`metricbeat-opw`](namespaces/metricbeat-opw/) | Metricbeat + OPW | Elasticsearch, Metricbeat DaemonSet, Vector (OPW) |
| [`monitoring`](namespaces/monitoring/) | Monitoring Stack | Prometheus, Grafana, demo-api/frontend/database, alertmanager |
| [`observability-pipelines`](namespaces/observability-pipelines/) | OPW Worker | Vector 0.43 — filters stock-app logs → Flex Logs |
| [`stock-app`](namespaces/stock-app/) | Stock Trading App | Flask stock-backend, nginx frontend, PostgreSQL |
| [`yes001`](namespaces/yes001/) | YES TV RUM Demo | RUM beforeSend filtering demo for yes.co.il |

## Helm Releases

| Release | Namespace | Chart | Version |
|---|---|---|---|
| `cloudprem` | cloudprem | cloudprem | 0.2.2 |
| `cloudprem-postgres` | cloudprem | postgresql | 18.5.6 |
| `datadog-operator` | default | datadog-operator | 2.17.0 |
| `arc` | githubci | actions-runner-controller | 0.23.7 |
| `grafana` | monitoring | grafana | 10.5.15 |
| `prometheus` | monitoring | prometheus | 28.9.1 |
| `karpenter` | kube-system | karpenter | 1.8.6 |
| `kube-ovn` | kube-system | kube-ovn | v1.15.4 |
| `vpa` | kube-system | vertical-pod-autoscaler | 11.1.1 |

## AWS Infrastructure

| Resource | Value |
|---|---|
| S3 Bucket (CloudPrem) | `eran-cloudprem-karpenter` |
| IAM Role (CloudPrem IRSA) | `eran-cloudprem-role` |
| IAM Role (EBS CSI IRSA) | `eran-karpenter-ebs-csi-role` |
| EBS StorageClass | `gp2` (default) |
| LB (CloudPrem external) | `a64fdc58544e54ebc8ba109bad172109-1032625977.us-east-1.elb.amazonaws.com:7280` |
| LB (stock-frontend) | `ac698cd2fad9c4cdd91b4c96151c4e26-31912dcb4ab82c12.elb.us-east-1.amazonaws.com` |

## Restore Instructions

### Prerequisites
```bash
# 1. Configure AWS credentials
export AWS_ACCESS_KEY_ID=...
export AWS_SECRET_ACCESS_KEY=...
export AWS_SESSION_TOKEN=...

# 2. Connect to cluster
aws eks update-kubeconfig --name karpenter-demo --region us-east-1

# 3. Verify
kubectl get nodes
```

### Restore a namespace
```bash
# Apply all manifests for a namespace
kubectl apply -f namespaces/<namespace>/

# Example: restore stock-app
kubectl apply -f namespaces/stock-app/
```

### Restore Helm releases
```bash
helm repo add datadog https://helm.datadoghq.com
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo add actions-runner-controller https://actions-runner-controller.github.io/actions-runner-controller
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo add karpenter https://charts.karpenter.sh
helm repo add kubeovn https://kubeovn.github.io/kube-ovn/
helm repo update

# See docs/helm-releases.txt for exact versions
```

### Secrets
> ⚠️ Secrets are NOT stored in this repo. Re-create them manually before applying manifests.

| Secret | Namespace | Keys |
|---|---|---|
| `datadog-secret` | default, cloudprem, datadog | `api-key` |
| `cloudprem-metastore-uri` | cloudprem | `QW_METASTORE_URI` |
| GitHub PAT | githubci | see `arc` helm values |

## Key Config Notes

### CloudPrem
- Logs stored in S3: `s3://eran-cloudprem-karpenter/indexes/`
- Agents route logs via `DD_LOGS_CONFIG_LOGS_DD_URL=http://cloudprem-indexer.cloudprem.svc.cluster.local:7280`
- `DD_OBSERVABILITY_PIPELINES_WORKER_LOGS_ENABLED=false` (disabled to allow CloudPrem routing)

### Kube-OVN
- Pods get IPs from `10.16.0.0/16` overlay network
- IP preservation for live migration: annotate pod with `ovn.kubernetes.io/ip_address`

### Karpenter
- NodePool: spot instances (t3.large, t3.xlarge, t3.2xlarge, c7i-flex.large)
- Zone: us-east-1c
