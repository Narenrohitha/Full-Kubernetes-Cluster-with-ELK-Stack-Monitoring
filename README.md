<div align="center">

# ☸️ Kubernetes Cluster + ELK Stack Monitoring

### *From zero to full observability — on bare Ubuntu 24.04*

---

![](https://img.shields.io/badge/Kubernetes-v1.30.3-326CE5?style=flat-square&logo=kubernetes&logoColor=white)
![](https://img.shields.io/badge/Elasticsearch-8.5.1-005571?style=flat-square&logo=elasticsearch&logoColor=white)
![](https://img.shields.io/badge/Kibana-8.5.1-005571?style=flat-square&logo=kibana&logoColor=white)
![](https://img.shields.io/badge/Logstash-Filebeat-005571?style=flat-square&logo=logstash&logoColor=white)
![](https://img.shields.io/badge/Ubuntu-24.04_LTS-E95420?style=flat-square&logo=ubuntu&logoColor=white)

![](https://img.shields.io/badge/Runtime-containerd-575757?style=flat-square&logo=containerd&logoColor=white)
![](https://img.shields.io/badge/Network-Calico_v3.25-F8821A?style=flat-square)
![](https://img.shields.io/badge/Helm-3.x-0F1689?style=flat-square&logo=helm&logoColor=white)
![](https://img.shields.io/badge/Nodes-3_(1_master_+_2_workers)-00C851?style=flat-square)
![](https://img.shields.io/badge/Logs-1%2C432%2B_per_15_min-blueviolet?style=flat-square)

---

**3-node Kubernetes cluster. Full ELK observability. Real commands. Real logs. No cloud required.**

[📖 Read the Full Blog Post on Medium](https://medium.com/@narengl2001/i-built-a-full-kubernetes-cluster-with-elk-stack-monitoring-on-ubuntu-24-04-heres-exactly-how-d6efb7eb894a)

</div>

---

## 📸 What This Looks Like When Running

```
┌─── Cluster Dashboard ─────────────────────────────────────────────────┐
│                                                                        │
│   NAME           STATUS    ROLES            VERSION                   │
│   master-node    Ready     control-plane    v1.30.3                   │
│   worker-1       Ready     <none>           v1.30.3                   │
│   worker-2       Ready     <none>           v1.30.3                   │
│                                                                        │
│   Pods: 8 Running   Deployments: 4   Services: 3   Restarts: 0        │
└────────────────────────────────────────────────────────────────────────┘

┌─── Kibana Discover ────────────────────────────────────────────────────┐
│                                                                        │
│   Index: filebeat-*          Time: Last 15 minutes                    │
│                                                                        │
│   ▂▃▄▅▆▆▇▇██▇▇▆▅▄▃▄▅▆▇██▇▆▅▃▂▂▃▄▅▆▇█   ← log histogram              │
│                                                                        │
│   Hits: 1,432   Avg: ~95/min   Source: all pods                       │
└────────────────────────────────────────────────────────────────────────┘

┌─── Custom Kibana Dashboard ────────────────────────────────────────────┐
│                                                                        │
│   Portfolio Requests              Pod CPU Usage (Metricbeat)           │
│   ┌──────────────────────┐        ┌──────────────────────┐            │
│   │    ▂▄▆█▇▅▃▄▆█▇▆▄    │        │  portfolio-xxx ──╮   │            │
│   │                      │        │  portfolio-yyy ──┤   │            │
│   └──────────────────────┘        └──────────────────────┘            │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 🗺️ Architecture

```
                          ┌─────────────────────────────────────────┐
                          │         Ubuntu 24.04 — 3 Nodes          │
                          └─────────────────────────────────────────┘
                                           │
              ┌────────────────────────────┼────────────────────────────┐
              │                            │                            │
    ┌─────────▼─────────┐       ┌──────────▼──────────┐     ┌──────────▼──────────┐
    │    master-node     │       │      worker-1        │     │      worker-2        │
    │  ─────────────    │       │  ─────────────────   │     │  ─────────────────   │
    │  api-server        │       │  portfolio (rep 1)   │     │  portfolio (rep 2+3) │
    │  etcd              │       │  filebeat            │     │  filebeat            │
    │  scheduler         │       │  metricbeat          │     │  metricbeat          │
    │  controller-mgr    │       └──────────────────────┘     └──────────────────────┘
    └────────────────────┘
              │                            │                            │
              └────────────────────────────┴────────────────────────────┘
                                           │
                               ┌───────────▼────────────┐
                               │    Calico CNI Network   │
                               │    (pod-to-pod mesh)    │
                               └───────────┬────────────┘
                                           │
                          ┌────────────────▼────────────────────┐
                          │       namespace: logging             │
                          │                                      │
                          │  ┌──────────────┐  ┌─────────────┐  │
                          │  │Elasticsearch │  │   Kibana    │  │
                          │  │  :9200 (int) │◄─│  :30601     │  │
                          │  └──────┬───────┘  └─────────────┘  │
                          │         │                            │
                          │    filebeat-*  ←  all pod logs       │
                          │    metricbeat-* ← CPU / memory       │
                          └──────────────────────────────────────┘

  App URL  →  http://<WORKER_IP>:30080
  Kibana   →  http://<WORKER_IP>:30601
```

---

## ✅ What You'll Have

| # | Component | Detail |
|---|-----------|--------|
| 1 | ☸️ Kubernetes Cluster | 1 master + 2 workers — all `Ready` |
| 2 | 🐳 Container Runtime | containerd with SystemdCgroup |
| 3 | 🌐 CNI | Calico v3.25.0 — pod networking |
| 4 | 🚀 App | Portfolio site — 3 replicas, NodePort :30080 |
| 5 | 🔍 Elasticsearch | Single node, namespace `logging` |
| 6 | 📊 Kibana | Dashboards + Discover, NodePort :30601 |
| 7 | 📋 Filebeat | DaemonSet — every container log |
| 8 | 📈 Metricbeat | DaemonSet — CPU, memory, network |

---

## 🖥️ Prerequisites

| Resource | Spec |
|----------|------|
| Servers | 3× Ubuntu 24.04 LTS |
| CPU | 2+ cores per node |
| RAM | 4 GB per node (8 GB recommended) |
| Disk | 20 GB free on `/var` |
| Access | `root` or `sudo` on all 3 nodes |

---

# PART 1 — Kubernetes Cluster

---

## ◆ Step 1 — Prepare All Nodes
> 🔁 Run on **master + all workers**

```bash
# Update system packages
sudo apt update && sudo apt upgrade -y

# Disable swap — Kubernetes requires this
sudo swapoff -a
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
```

---

## ◆ Step 2 — Kernel Modules & Sysctl
> 🔁 Run on **master + all workers**

```bash
sudo tee /etc/modules-load.d/containerd.conf <<EOF
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter

sudo tee /etc/sysctl.d/kubernetes.conf <<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables  = 1
net.ipv4.ip_forward                 = 1
EOF

sudo sysctl --system
```

> 💡 `bridge-nf-call-iptables` routes traffic between pods across nodes.
> Skip it and inter-pod communication **silently fails.**

---

## ◆ Step 3 — Install containerd
> 🔁 Run on **master + all workers**

```bash
sudo apt install -y curl gnupg2 software-properties-common \
  apt-transport-https ca-certificates

sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
  sudo gpg --dearmour -o /etc/apt/trusted.gpg.d/docker.gpg

sudo add-apt-repository \
  "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

sudo apt update && sudo apt install -y containerd.io

# ── CRITICAL ──────────────────────────────────────────────────────────
# Enable systemd cgroup driver — missing this = kubelet crash-loop
containerd config default | sudo tee /etc/containerd/config.toml >/dev/null 2>&1
sudo sed -i 's/SystemdCgroup \= false/SystemdCgroup \= true/g' \
  /etc/containerd/config.toml
# ─────────────────────────────────────────────────────────────────────

sudo systemctl restart containerd
sudo systemctl enable containerd
```

> ⚠️ **#1 most common mistake:** Forgetting `SystemdCgroup = true`.
> This causes kubelet to crash-loop. Set it **before** cluster init.

---

## ◆ Step 4 — Install Kubernetes Components
> 🔁 Run on **master + all workers**

```bash
sudo mkdir -p /etc/apt/keyrings

echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] \
  https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /" | \
  sudo tee /etc/apt/sources.list.d/kubernetes.list

curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | \
  sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

sudo apt update
sudo apt install -y kubelet kubeadm kubectl

# Lock versions — prevent accidental upgrades
sudo apt-mark hold kubelet kubeadm kubectl
```

---

## ◆ Step 5 — Initialize Master Node
> 🎯 Run on **master only**

```bash
sudo kubeadm init

# Configure kubectl access
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

> 📋 **Save the join command** printed in the output. It looks like:
> ```
> kubeadm join <MASTER_IP>:6443 --token <TOKEN> \
>   --discovery-token-ca-cert-hash sha256:<HASH>
> ```
> You'll need it for the worker nodes in the next step.

---

## ◆ Step 6 — Join Worker Nodes
> 🎯 Run on **each worker**

```bash
# Paste the join command from the previous step
kubeadm join <MASTER_IP>:6443 --token <TOKEN> \
  --discovery-token-ca-cert-hash sha256:<HASH>
```

---

## ◆ Step 7 — Install Calico Networking
> 🎯 Run on **master**

```bash
kubectl apply -f \
  https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/calico.yaml

# Wait ~2 minutes, then verify
kubectl get nodes
```

**Expected output:**

```
NAME          STATUS   ROLES           AGE   VERSION
master-node   Ready    control-plane   10m   v1.30.3   ✅
worker-1      Ready    <none>           5m   v1.30.3   ✅
worker-2      Ready    <none>           5m   v1.30.3   ✅
```

---

# PART 2 — Deploy the Application

---

## ◆ Step 8 — The Portfolio App

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>My Kubernetes Cluster</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      color: white; min-height: 100vh;
      display: flex; align-items: center;
      justify-content: center; text-align: center;
    }
    .stats { display: grid; grid-template-columns: repeat(4,1fr); gap: 20px; margin-top: 40px; }
    .card {
      background: rgba(255,255,255,0.12); backdrop-filter: blur(10px);
      border-radius: 20px; padding: 30px;
      border: 1px solid rgba(255,255,255,0.2);
    }
    .card h3 { font-size: 2.8em; color: #ffd700; }
  </style>
</head>
<body>
  <div>
    <h1>☸️ My Kubernetes Cluster</h1>
    <p>A Production-Ready Container Orchestration Platform</p>
    <div class="stats">
      <div class="card"><h3>3</h3><p>Nodes</p></div>
      <div class="card"><h3>8</h3><p>Pods</p></div>
      <div class="card"><h3>4</h3><p>Deployments</p></div>
      <div class="card"><h3>3</h3><p>Services</p></div>
    </div>
  </div>
</body>
</html>
```

---

## ◆ Step 9 — Build & Push Docker Image

```bash
# Dockerfile
cat > Dockerfile <<'EOF'
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
EOF

docker build -t portfolio-site:latest .
docker tag portfolio-site:latest <yourusername>/portfolio-site:latest
docker login
docker push <yourusername>/portfolio-site:latest
```

---

## ◆ Step 10 — Deploy to Kubernetes

```yaml
# deploy.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: portfolio-site
  labels:
    app: portfolio
spec:
  replicas: 3
  selector:
    matchLabels:
      app: portfolio
  template:
    metadata:
      labels:
        app: portfolio
    spec:
      containers:
      - name: portfolio
        image: <yourusername>/portfolio-site:latest
        imagePullPolicy: Always
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: portfolio-service
spec:
  type: NodePort
  selector:
    app: portfolio
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30080
```

```bash
kubectl apply -f deploy.yaml
echo "App live at → http://<WORKER_NODE_IP>:30080"
```

---

# PART 3 — ELK Stack via Helm

---

## ◆ Step 11 — Install Helm

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
helm version
```

---

## ◆ Step 12 — CRITICAL: vm.max_map_count
> 🔁 Run on **ALL nodes** — Elasticsearch won't start without this

```bash
sudo sysctl -w vm.max_map_count=262144
echo "vm.max_map_count=262144" | sudo tee -a /etc/sysctl.conf
```

---

## ◆ Step 13 — Add Elastic Repo + Create Namespace

```bash
helm repo add elastic https://helm.elastic.co
helm repo update
kubectl create namespace logging
```

---

## ◆ Step 14 — Install Elasticsearch

```yaml
# elasticsearch-minimal.yaml  (optimised for <8 GB RAM nodes)
replicas: 1
minimumMasterNodes: 1
persistence:
  enabled: false
resources:
  requests:
    cpu: "200m"
    memory: "512Mi"
  limits:
    memory: "1Gi"
esJavaOpts: "-Xmx512m -Xms512m"
tolerations:
  - effect: NoSchedule
    operator: Exists
```

```bash
helm install elasticsearch elastic/elasticsearch \
  --namespace logging \
  --values elasticsearch-minimal.yaml

# Wait for ready before continuing
kubectl rollout status statefulset/elasticsearch-master -n logging
```

---

## ◆ Step 15 — Retrieve Auto-Generated Password

```bash
PASSWORD=$(kubectl get secret -n logging elasticsearch-master-credentials \
  -o jsonpath="{.data.password}" | base64 -d)

echo "┌─────────────────────────────────────────┐"
echo "│  Elasticsearch Password: $PASSWORD"
echo "└─────────────────────────────────────────┘"
# Save this — used in all configs below
```

---

## ◆ Step 16 — Install Kibana

```bash
helm install kibana elastic/kibana \
  --namespace logging \
  --set service.type=NodePort \
  --set service.nodePort=30601 \
  --set elasticsearchHosts="https://elasticsearch-master:9200"
```

---

## ◆ Step 17 — Install Filebeat (Log Collection)

```yaml
# filebeat-config.yaml  ← replace $PASSWORD with your actual password
daemonset:
  enabled: true

filebeatConfig:
  filebeat.yml: |
    filebeat.inputs:
      - type: container
        paths:
          - /var/log/containers/*.log
        processors:
          - add_kubernetes_metadata:
              host: ${NODE_NAME}

    output.elasticsearch:
      hosts: ["https://elasticsearch-master:9200"]
      username: "elastic"
      password: "$PASSWORD"
      ssl.verification_mode: "none"

    setup.kibana:
      host: "kibana-kibana:5601"
```

```bash
helm install filebeat elastic/filebeat \
  --namespace logging \
  --values filebeat-config.yaml
```

---

## ◆ Step 18 — Install Metricbeat (Metrics Collection)

```yaml
# metricbeat-config.yaml  ← replace $PASSWORD with your actual password
daemonset:
  enabled: true

metricbeatConfig:
  metricbeat.yml: |
    metricbeat.modules:
    - module: kubernetes
      enabled: true
      hosts: ["https://${KUBERNETES_SERVICE_HOST}:${KUBERNETES_SERVICE_PORT}"]
      bearer_token_file: /var/run/secrets/kubernetes.io/serviceaccount/token
      ssl.verification_mode: none
      metricsets: [node, system, pod, container]
      period: 10s

    - module: system
      metricsets: [cpu, memory, network, filesystem]
      period: 10s

    output.elasticsearch:
      hosts: ["https://elasticsearch-master:9200"]
      username: "elastic"
      password: "$PASSWORD"
      ssl.verification_mode: "none"

    setup.kibana:
      host: "kibana-kibana:5601"
```

```bash
helm install metricbeat elastic/metricbeat \
  --namespace logging \
  --values metricbeat-config.yaml
```

---

## ◆ Step 19 — Verify Everything

```bash
kubectl get pods -n logging
```

```
NAME                             READY   STATUS    RESTARTS   AGE
elasticsearch-master-0           1/1     Running   0          5m   ✅
kibana-kibana-5d9f7c8b7-xxxxx    1/1     Running   0          3m   ✅
filebeat-filebeat-xxxxx          1/1     Running   0          2m   ✅
metricbeat-metricbeat-xxxxx      1/1     Running   0          2m   ✅
```

---

# PART 4 — Kibana Dashboards

---

## ◆ Step 20 — Access Kibana

```bash
NODE_IP=$(kubectl get nodes \
  -o jsonpath='{.items[1].status.addresses[0].address}')

echo "──────────────────────────────────────"
echo "  URL:      http://$NODE_IP:30601"
echo "  Username: elastic"
echo "  Password: $PASSWORD"
echo "──────────────────────────────────────"
```

---

## ◆ Step 21 — Create Index Patterns

`Stack Management` → `Index Patterns` → `Create index pattern`

| Pattern | Time Field |
|---------|------------|
| `filebeat-*` | `@timestamp` |
| `metricbeat-*` | `@timestamp` |

---

## ◆ Step 22 — Explore Live Logs

Go to **Discover** → select `filebeat-*`

Within minutes you'll see log entries flowing in. I had **1,432 hits in 15 minutes** across the whole cluster.

Filter to your app only:
```
kubernetes.labels.app : "portfolio"
```

---

## ◆ Step 23 — Build Your Dashboard

`Dashboard` → `Create dashboard` → `Add panel` → `Create visualization`

| Panel | Index | Chart Type | Key Config |
|-------|-------|------------|------------|
| 📊 Request Count | `filebeat-*` | Vertical Bar | X = Date Histogram · Y = Count |
| ⚡ Pod CPU | `metricbeat-*` | Line | Avg `kubernetes.pod.cpu.usage.node.pct` |
| 🧠 Memory | `metricbeat-*` | Area | Avg `kubernetes.pod.memory.usage.bytes` |
| 📋 Log Volume | `filebeat-*` | Metric | Count · Last 15 min |

---

# 🩺 Troubleshooting

---

<details>
<summary><b>🔴 kubelet crash-loops on startup</b></summary>

```bash
# Cause: SystemdCgroup = false in containerd config
sudo sed -i 's/SystemdCgroup \= false/SystemdCgroup \= true/g' \
  /etc/containerd/config.toml
sudo systemctl restart containerd
sudo systemctl restart kubelet
```
</details>

<details>
<summary><b>🔴 Elasticsearch pod stays Pending</b></summary>

```bash
# Cause: vm.max_map_count too low — run on ALL nodes
sudo sysctl -w vm.max_map_count=262144
echo "vm.max_map_count=262144" | sudo tee -a /etc/sysctl.conf

# Force reschedule
kubectl delete pod elasticsearch-master-0 -n logging
```
</details>

<details>
<summary><b>🔴 No logs appearing in Kibana</b></summary>

```bash
# Cause: wrong Elasticsearch password in Filebeat config
PASSWORD=$(kubectl get secret -n logging elasticsearch-master-credentials \
  -o jsonpath="{.data.password}" | base64 -d)

# Check Filebeat logs
kubectl logs -n logging -l app=filebeat --tail=50

# Re-install with correct password
helm upgrade filebeat elastic/filebeat \
  --namespace logging --values filebeat-config.yaml
```
</details>

<details>
<summary><b>🔴 Nodes stuck NotReady</b></summary>

```bash
# Re-apply Calico CNI
kubectl apply -f \
  https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/calico.yaml

kubectl get pods -n kube-system | grep calico
kubectl describe node <node-name> | grep -A10 Conditions
```
</details>

<details>
<summary><b>🔴 ErrImagePull on portfolio pods</b></summary>

```bash
# Push image first, then force redeploy
docker push <yourusername>/portfolio-site:latest
kubectl rollout restart deployment/portfolio-site
```
</details>

<details>
<summary><b>🔴 kubeadm join token expired</b></summary>

```bash
# Generate a fresh join command on master
kubeadm token create --print-join-command
```
</details>

---

# ✅ Final Verified State

```
╔══════════════════════════════════════════════════════════════╗
║              CLUSTER — FULLY OPERATIONAL                     ║
╠══════════════════════════════════════════════════════════════╣
║  ☸️  Kubernetes    v1.30.3    3 nodes       All Ready        ║
║  🐳  Runtime       containerd  SystemdCgroup = true          ║
║  🌐  Network       Calico      v3.25.0      All pods meshed  ║
║  📦  Pods          8 running   0 restarts   0 pending        ║
║  📋  Log rate      1,432 hits  per 15-min   Filebeat ✅       ║
║  📈  Metrics       CPU+Memory  10s interval Metricbeat ✅     ║
║  📊  Kibana        Live        v8.5.1       Dashboards up    ║
╚══════════════════════════════════════════════════════════════╝
```

---

## 🔗 More From This Series

| Article | What's Inside |
|---------|--------------|
| [**K8s + ELK on Ubuntu 24.04**](https://medium.com/@narengl2001) | This guide |
| [**Pod Troubleshooting Field Guide**](https://medium.com/@narengl2001) | Every scheduling & runtime error with real kubectl fixes |
| [**50 Interview Questions + 24 Scenarios**](https://medium.com/@narengl2001) | Crack any Kubernetes interview |
| [**WordPress 5-Server Infra**](https://medium.com/@narengl2001) | Nginx LB · ELK · Prometheus · Grafana |

---

## 🤝 Contributing

1. Fork the repo
2. Create your branch — `git checkout -b feature/your-feature`
3. Commit — `git commit -m 'Add your feature'`
4. Push — `git push origin feature/your-feature`
5. Open a Pull Request

---

## 📄 License

MIT License — see [LICENSE](LICENSE) for details.

---

<div align="center">

---

### ⭐ If this saved you hours of debugging — star the repo

*It helps other engineers find it when they need it most.*

---

**Built by [Naren](https://medium.com/@narengl2001)**
*Cloud & DevSecOps Engineer — Building infrastructure, breaking it, fixing it, then writing about it.*

[![Medium](https://img.shields.io/badge/Follow_on_Medium-000000?style=flat-square&logo=medium&logoColor=white)](https://medium.com/@narengl2001)

</div>
