
---

# 📊 Installing Prometheus and Grafana via Helm

This guide outlines how to install the **Prometheus** and **Grafana** monitoring stack in your Kubernetes cluster using the `kube-prometheus-stack` Helm chart.

---

## 🔧 Prerequisites

- A running Kubernetes cluster (e.g., EKS, AKS, GKE, Minikube)
- `kubectl` configured to access the cluster
- Helm installed and initialized

---

## 📥 Step 1: Add the Helm Chart Repository

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

---

## 🚀 Step 2: Deploy the Chart into a New Namespace

1. Create the `monitoring` namespace:

   ```bash
   kubectl create ns monitoring
   ```

3. Install the chart with custom values:

   ```bash
   helm install monitoring prometheus-community/kube-prometheus-stack \
     -n monitoring \
     -f ./value.yaml
   ```

---

## ✅ Step 3: Verify the Installation

List all resources in the monitoring namespace:

```bash
kubectl get all -n monitoring
```

---

## 🌐 Step 4: Access the UIs

### Prometheus UI

```bash
kubectl port-forward service/prometheus-operated -n monitoring 9090:9090
```

> **Note:** If running on EC2 or any cloud VM, append `--address 0.0.0.0` to expose the port externally:
> ```bash
> kubectl port-forward --address 0.0.0.0 service/prometheus-operated -n monitoring 9090:9090
> ```
> Access via `http://<instance-ip>:9090`

---

### Grafana UI

```bash
kubectl port-forward service/monitoring-grafana -n monitoring 8080:80
```

- Access via `http://localhost:8080`
- **Default credentials**:
  - Username: `admin`
  - Password: `prom-operator`

---

### Alertmanager UI

```bash
kubectl port-forward service/alertmanager-operated -n monitoring 9093:9093
```

> Access via `http://localhost:9093`

---

## 🧼 Step 5: Clean Up

Uninstall the Helm release:

```bash
helm uninstall monitoring --namespace monitoring
```

Delete the monitoring namespace:

```bash
kubectl delete ns monitoring
```


---