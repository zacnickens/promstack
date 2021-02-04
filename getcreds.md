## Get creds for Prometheus

```bash
GRAFANA_USERNAME="$(kubectl get secret kube-prometheus-stack-grafana \
                      --namespace monitoring \
                      --output=jsonpath='{.data.admin-user}' | base64 --decode)"
GRAFANA_PASSWORD="$(kubectl get secret kube-prometheus-stack-grafana \
                      --namespace monitoring \
                      --output=jsonpath='{.data.admin-password}' | base64 --decode)"
echo "Grafana credentials:"
echo "- user: ${GRAFANA_USERNAME}"
echo "- pass: ${GRAFANA_PASSWORD}"
```

## Install Nexus3 OSS via helm

```bash
helm install nexus3 stevehipwell/nexus3 --version 4.0.2
```

## Patch Services to access UI

### Grafana
```bash
kubectl patch svc "kube-prometheus-stack-grafana" \\n  --namespace "monitoring" \\n  -p '{"spec": {"type": "LoadBalancer"}}'
```

### Nexus
```bash
kubectl patch svc "nexus3" \\n  --namespace "nexus" \\n  -p '{"spec": {"type": "LoadBalancer"}}'
```

