<div align="center">

<!-- ANIMATED WAVE HEADER -->
<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=6,11,20&height=220&section=header&text=Kubernetes%20%2B%20ELK%20Stack&fontSize=52&fontColor=fff&fontAlignY=40&desc=Production-Grade%20Monitoring%20on%20Ubuntu%2024.04&descAlignY=60&descColor=a5f3fc&animation=fadeIn&stroke=00d4ff&strokeWidth=1" width="100%"/>

<!-- TYPING ANIMATION -->
<img src="https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=700&size=24&pause=1200&color=00D4FF&center=true&vCenter=true&width=800&lines=☸️+3-Node+Kubernetes+Cluster+%7C+v1.30.3;📊+Full+ELK+Stack+via+Helm+%7C+v8.5.1;📋+1%2C432%2B+log+entries+per+15+minutes;⚡+Filebeat+%2B+Metricbeat+DaemonSets;🔍+Real-Time+Kibana+Dashboards+%7C+Live!" alt="Typing SVG"/>

<br/><br/>

<!-- TECH STACK BADGES ROW 1 -->
<img src="https://img.shields.io/badge/Kubernetes-v1.30.3-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white"/>
<img src="https://img.shields.io/badge/Elasticsearch-8.5.1-005571?style=for-the-badge&logo=elasticsearch&logoColor=white"/>
<img src="https://img.shields.io/badge/Kibana-8.5.1-005571?style=for-the-badge&logo=kibana&logoColor=white"/>
<img src="https://img.shields.io/badge/Ubuntu-24.04_LTS-E95420?style=for-the-badge&logo=ubuntu&logoColor=white"/>

<br/>

<!-- TECH STACK BADGES ROW 2 -->
<img src="https://img.shields.io/badge/Helm-3.x-0F1689?style=for-the-badge&logo=helm&logoColor=white"/>
<img src="https://img.shields.io/badge/containerd-Runtime-575757?style=for-the-badge&logo=containerd&logoColor=white"/>
<img src="https://img.shields.io/badge/Calico-CNI_v3.25-F8821A?style=for-the-badge&logo=tigera&logoColor=white"/>
<img src="https://img.shields.io/badge/Filebeat-Log_Shipper-00BFB3?style=for-the-badge&logo=elastic&logoColor=white"/>
<img src="https://img.shields.io/badge/Metricbeat-Metrics-EE4B9D?style=for-the-badge&logo=elastic&logoColor=white"/>

<br/><br/>

<!-- LIVE STATS BADGES -->
<img src="https://img.shields.io/badge/Nodes-3_%28Ready%29-00C851?style=for-the-badge&logo=kubernetes&logoColor=white"/>
<img src="https://img.shields.io/badge/Pods-8_Running-00C851?style=for-the-badge&logo=docker&logoColor=white"/>
<img src="https://img.shields.io/badge/Log_Rate-1%2C432%2B_%2F_15min-blueviolet?style=for-the-badge&logo=elasticsearch&logoColor=white"/>
<img src="https://img.shields.io/badge/Status-Production_Ready-success?style=for-the-badge"/>

<br/><br/>

<!-- MEDIUM BUTTON -->
<a href="https://medium.com/@narengl2001/i-built-a-full-kubernetes-cluster-with-elk-stack-monitoring-on-ubuntu-24-04-heres-exactly-how-d6efb7eb894a">
  <img src="https://img.shields.io/badge/📖_Read_Full_Blog_on_Medium-000000?style=for-the-badge&logo=medium&logoColor=white"/>
</a>

</div>

---

<div align="center">

## 🎯 What This Builds

</div>

```
 ╔══════════╗    ╔══════════════╗    ╔══════════════╗    ╔══════════════╗
 ║  3 Node  ║ →  ║   Portfolio  ║ →  ║  ELK Stack   ║ →  ║  Real-Time   ║
 ║  K8s     ║    ║  App Live    ║    ║  via Helm    ║    ║  Dashboards  ║
 ║ Cluster  ║    ║  :30080      ║    ║  :30601      ║    ║  1432+ logs  ║
 ╚══════════╝    ╚══════════════╝    ╚══════════════╝    ╚══════════════╝
```

> **Most Kubernetes tutorials stop at "your pods are running."**
> This goes further — giving you complete visibility into *what's happening inside* those pods.

---

<div align="center">

## 🏗️ Full Architecture

</div>

```
┌──────────────────────────────────────────────────────────────────────────┐
│                       UBUNTU 24.04 LTS — 3 NODES                        │
│                                                                          │
│  ┌─────────────────┐   ┌──────────────────┐   ┌──────────────────────┐  │
│  │   master-node   │   │    worker-1      │   │      worker-2        │  │
│  │─────────────────│   │──────────────────│   │──────────────────────│  │
│  │ • api-server    │   │ • portfolio:1    │   │ • portfolio:2        │  │
│  │ • etcd          │   │ • filebeat       │   │ • portfolio:3        │  │
│  │ • scheduler     │   │ • metricbeat     │   │ • filebeat           │  │
│  │ • ctrl-manager  │   │                  │   │ • metricbeat         │  │
│  └────────┬────────┘   └────────┬─────────┘   └──────────┬───────────┘  │
│           │                     │                        │              │
│           └─────────────────────┼────────────────────────┘              │
│                        ┌────────▼────────┐                              │
│                        │  CALICO  CNI    │                              │
│                        │  pod network    │                              │
│                        └────────┬────────┘                              │
│                                 │                                        │
│            ┌────────────────────▼───────────────────────┐               │
│            │           namespace: logging                │               │
│            │  ┌──────────────────┐  ┌────────────────┐  │               │
│            │  │  Elasticsearch   │  │    Kibana      │  │               │
│            │  │  StatefulSet     │◄─│  Deployment    │  │               │
│            │  │  port: 9200      │  │  NodePort:     │  │               │
│            │  │                  │  │  30601         │  │               │
│            │  └────────┬─────────┘  └────────────────┘  │               │
│            │           │                                  │               │
│            │  filebeat-*   ← pod logs (all containers)   │               │
│            │  metricbeat-* ← CPU · memory · network      │               │
│            └──────────────────────────────────────────────┘               │
└──────────────────────────────────────────────────────────────────────────┘

     App → http://<WORKER_IP>:30080        KB → http://<WORKER_IP>:30601
```

---

<div align="center">

## 🛠️ Tech Stack

</div>

<div align="center">

<!-- SKILL ICONS - renders as beautiful icons grid -->
<img src="https://skillicons.dev/icons?i=kubernetes,linux,docker,bash,nginx,githubactions&theme=dark&perline=6"/>

</div>

<br/>

<div align="center">

| Layer | Technology | Version | Role |
|:---:|:---:|:---:|:---|
| 🔵 | **Kubernetes** | v1.30.3 | Container orchestration |
| 🟠 | **Ubuntu** | 24.04 LTS | Host OS — all 3 nodes |
| ⚫ | **containerd** | latest | Container runtime |
| 🟡 | **Calico** | v3.25.0 | Pod networking (CNI) |
| 🔷 | **Helm** | 3.x | Package manager for K8s |
| 🟦 | **Elasticsearch** | 8.5.1 | Log + metric storage |
| 🟣 | **Kibana** | 8.5.1 | Visualization & dashboards |
| 🟢 | **Filebeat** | 8.5.1 | Log shipper (DaemonSet) |
| 🔴 | **Metricbeat** | 8.5.1 | Metrics collector (DaemonSet) |

</div>

---

<div align="center">

## 📸 Live Dashboard Preview

</div>

```
 ┌── kubectl get nodes ─────────────────────────────────────────────────┐
 │                                                                      │
 │  NAME          STATUS   ROLES           AGE   VERSION                │
 │  master-node   Ready    control-plane   10m   v1.30.3   ✅           │
 │  worker-1      Ready    <none>          5m    v1.30.3   ✅           │
 │  worker-2      Ready    <none>          5m    v1.30.3   ✅           │
 │                                                                      │
 └──────────────────────────────────────────────────────────────────────┘

 ┌── Kibana Discover — filebeat-* ──────────────────────────────────────┐
 │                                                                      │
 │  Time range: Last 15 minutes                Hits: 1,432  🔥          │
 │                                                                      │
 │  ▁▂▃▄▅▆▆▇██▇▇▆▅▄▃▄▅▆▆▇▇██▇▅▄▃▂▂▃▄▅▆▇██▇▆▅  histogram               │
 │                                                                      │
 │  kubernetes.labels.app: "portfolio"         Pods: all ✅             │
 └──────────────────────────────────────────────────────────────────────┘

 ┌── Custom Kibana Dashboard ───────────────────────────────────────────┐
 │                                                                      │
 │  📊 Request Count (Bar)       ⚡ Pod CPU Usage (Line)                │
 │  ┌──────────────────────┐     ┌───────────────────────────┐          │
 │  │  ▂ ▄ █ █ ▇ ▅ ▄ █ ▇  │     │  pod-1 ━━━━━━╮            │          │
 │  │                      │     │  pod-2 ╌╌╌╌╌╌┤ 2.4%       │          │
 │  └──────────────────────┘     └───────────────────────────┘          │
 │                                                                      │
 │  🧠 Memory Usage (Area)       📋 Log Volume (Metric)                 │
 │  ┌──────────────────────┐     ┌───────────────────────────┐          │
 │  │ ░░▒▒▓███▓▒▒░░▒▒▓███  │     │                           │          │
 │  │ Avg: 128 MB / pod    │     │        1,432              │          │
 │  └──────────────────────┘     │     logs / 15 min         │          │
 │                               └───────────────────────────┘          │
 └──────────────────────────────────────────────────────────────────────┘
```

---

<div align="center">

## ⚡ Prerequisites

</div>

<div align="center">

| 🖥️ Resource | 📋 Minimum | ✅ Recommended |
|:---:|:---:|:---:|
| Servers | 3× Ubuntu 24.04 LTS | 3× Ubuntu 24.04 LTS |
| CPU | 2 cores / node | 4 cores / node |
| RAM | 4 GB / node | 8 GB / node |
| Disk | 20 GB on `/var` | 50 GB on `/var` |
| Access | `sudo` on all nodes | `root` on all nodes |

</div>

---

<div align="center">

# 🚀 PART 1 — Kubernetes Cluster Setup

</div>

---

### `STEP 01` — Prepare All Nodes
> 🔁 **Run on every node** (master + both workers)

```bash
# ─── Update system ─────────────────────────────────────────
sudo apt update && sudo apt upgrade -y

# ─── Disable swap (Kubernetes requires this) ───────────────
sudo swapoff -a
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
```

---

### `STEP 02` — Kernel Modules & Network Sysctl
> 🔁 **Run on every node**

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

> 💡 `bridge-nf-call-iptables` lets iptables see bridged traffic.
> Without it, pod-to-pod communication **silently fails.**

---

### `STEP 03` — Install containerd
> 🔁 **Run on every node**

```bash
sudo apt install -y curl gnupg2 software-properties-common \
  apt-transport-https ca-certificates

sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
  sudo gpg --dearmour -o /etc/apt/trusted.gpg.d/docker.gpg

sudo add-apt-repository \
  "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

sudo apt update && sudo apt install -y containerd.io

# ╔══════════════════════════════════════════════════════════╗
# ║  CRITICAL: enable systemd cgroup driver                 ║
# ║  Skipping this = kubelet crash-loop on startup          ║
# ╚══════════════════════════════════════════════════════════╝
containerd config default | sudo tee /etc/containerd/config.toml >/dev/null 2>&1
sudo sed -i 's/SystemdCgroup \= false/SystemdCgroup \= true/g' \
  /etc/containerd/config.toml

sudo systemctl restart containerd && sudo systemctl enable containerd
```

> ⚠️ **#1 Most Common Mistake** — Forgetting `SystemdCgroup = true`.
> Always set this **before** running `kubeadm init`.

---

### `STEP 04` — Install Kubernetes Components
> 🔁 **Run on every node**

```bash
sudo mkdir -p /etc/apt/keyrings

echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] \
  https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /" | \
  sudo tee /etc/apt/sources.list.d/kubernetes.list

curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | \
  sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

sudo apt update
sudo apt install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl  # 🔒 lock versions
```

---

### `STEP 05` — Initialize Master Node
> 🎯 **Master node only**

```bash
sudo kubeadm init

# ── Configure kubectl ──────────────────────────────────────
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

> 📋 **SAVE THE JOIN COMMAND** from the output — needed for workers:
> ```
> kubeadm join <MASTER_IP>:6443 --token <TOKEN> \
>   --discovery-token-ca-cert-hash sha256:<HASH>
> ```

---

### `STEP 06` — Join Worker Nodes
> 🎯 **Each worker node**

```bash
kubeadm join <MASTER_IP>:6443 --token <TOKEN> \
  --discovery-token-ca-cert-hash sha256:<HASH>
```

---

### `STEP 07` — Install Calico Networking
> 🎯 **Master node**

```bash
kubectl apply -f \
  https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/calico.yaml

# ── Verify all nodes Ready (wait ~2 min) ──────────────────
kubectl get nodes
```

```
NAME          STATUS   ROLES           VERSION
master-node   Ready    control-plane   v1.30.3   ✅
worker-1      Ready    <none>          v1.30.3   ✅
worker-2      Ready    <none>          v1.30.3   ✅
```

---

<div align="center">

# 🚀 PART 2 — Deploy the Application

</div>

---

### `STEP 08` — Build the Portfolio App

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>My Kubernetes Cluster</title>
  <style>
    body {
      font-family:'Segoe UI',sans-serif;
      background:linear-gradient(135deg,#667eea 0%,#764ba2 100%);
      color:white;min-height:100vh;
      display:flex;align-items:center;justify-content:center;text-align:center;
    }
    .stats{display:grid;grid-template-columns:repeat(4,1fr);gap:20px;margin-top:40px}
    .card{
      background:rgba(255,255,255,0.12);backdrop-filter:blur(10px);
      border-radius:20px;padding:30px;border:1px solid rgba(255,255,255,0.2)
    }
    .card h3{font-size:2.8em;color:#ffd700}
  </style>
</head>
<body>
  <div>
    <h1>☸️ My Kubernetes Cluster</h1>
    <p>Production-Ready Container Orchestration Platform</p>
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

### `STEP 09` — Dockerize & Push

```bash
cat > Dockerfile <<'EOF'
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
EOF

docker build -t portfolio-site:latest .
docker tag  portfolio-site:latest <yourusername>/portfolio-site:latest
docker login && docker push <yourusername>/portfolio-site:latest
```

---

### `STEP 10` — Deploy to Kubernetes

```yaml
# deploy.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: portfolio-site
  labels: { app: portfolio }
spec:
  replicas: 3
  selector:
    matchLabels: { app: portfolio }
  template:
    metadata:
      labels: { app: portfolio }
    spec:
      containers:
      - name: portfolio
        image: <yourusername>/portfolio-site:latest
        imagePullPolicy: Always
        ports: [{ containerPort: 80 }]
---
apiVersion: v1
kind: Service
metadata:
  name: portfolio-service
spec:
  type: NodePort
  selector: { app: portfolio }
  ports: [{ port: 80, targetPort: 80, nodePort: 30080 }]
```

```bash
kubectl apply -f deploy.yaml
# ✅ Live at → http://<WORKER_IP>:30080
```

---

<div align="center">

# 🚀 PART 3 — ELK Stack via Helm

</div>

---

### `STEP 11` — Install Helm

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

---

### `STEP 12` — CRITICAL: vm.max_map_count
> 🔁 **ALL nodes** — Elasticsearch refuses to start without this

```bash
sudo sysctl -w vm.max_map_count=262144
echo "vm.max_map_count=262144" | sudo tee -a /etc/sysctl.conf
```

---

### `STEP 13` — Add Elastic Helm Repo

```bash
helm repo add elastic https://helm.elastic.co && helm repo update
kubectl create namespace logging
```

---

### `STEP 14` — Install Elasticsearch

```yaml
# elasticsearch-minimal.yaml
replicas: 1
minimumMasterNodes: 1
persistence:
  enabled: false
resources:
  requests: { cpu: "200m", memory: "512Mi" }
  limits:   { memory: "1Gi" }
esJavaOpts: "-Xmx512m -Xms512m"
tolerations:
  - { effect: NoSchedule, operator: Exists }
```

```bash
helm install elasticsearch elastic/elasticsearch \
  --namespace logging --values elasticsearch-minimal.yaml

kubectl rollout status statefulset/elasticsearch-master -n logging
```

---

### `STEP 15` — Get the Password

```bash
PASSWORD=$(kubectl get secret -n logging elasticsearch-master-credentials \
  -o jsonpath="{.data.password}" | base64 -d)

echo "┌─────────────────────────────────────┐"
echo "│  🔑  Password: $PASSWORD             "
echo "└─────────────────────────────────────┘"
```

---

### `STEP 16` — Install Kibana

```bash
helm install kibana elastic/kibana \
  --namespace logging \
  --set service.type=NodePort \
  --set service.nodePort=30601 \
  --set elasticsearchHosts="https://elasticsearch-master:9200"
```

---

### `STEP 17` — Install Filebeat

```yaml
# filebeat-config.yaml  (replace $PASSWORD)
daemonset:
  enabled: true
filebeatConfig:
  filebeat.yml: |
    filebeat.inputs:
      - type: container
        paths: ["/var/log/containers/*.log"]
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
  --namespace logging --values filebeat-config.yaml
```

---

### `STEP 18` — Install Metricbeat

```yaml
# metricbeat-config.yaml  (replace $PASSWORD)
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
  --namespace logging --values metricbeat-config.yaml
```

---

### `STEP 19` — Verify All Pods

```bash
kubectl get pods -n logging
```

```
NAME                             READY   STATUS    AGE
elasticsearch-master-0           1/1     Running   5m   ✅
kibana-kibana-5d9f7c8b7-xxxxx    1/1     Running   3m   ✅
filebeat-filebeat-xxxxx          1/1     Running   2m   ✅
metricbeat-metricbeat-xxxxx      1/1     Running   2m   ✅
```

---

<div align="center">

# 🚀 PART 4 — Kibana Dashboards

</div>

---

### `STEP 20` — Access Kibana

```bash
NODE_IP=$(kubectl get nodes -o jsonpath='{.items[1].status.addresses[0].address}')
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
echo "  🌐  http://$NODE_IP:30601"
echo "  👤  elastic"
echo "  🔑  $PASSWORD"
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
```

---

### `STEP 21` — Index Patterns

`Stack Management` → `Index Patterns` → `Create index pattern`

| Pattern | Time Field |
|:---:|:---:|
| `filebeat-*` | `@timestamp` |
| `metricbeat-*` | `@timestamp` |

---

### `STEP 22` — Filter Your App Logs

```
kubernetes.labels.app : "portfolio"
```

---

### `STEP 23` — Dashboard Panels

| Panel | Index | Type | Config |
|:---:|:---:|:---:|:---|
| 📊 Request Count | `filebeat-*` | Bar | X=Date Histogram · Y=Count |
| ⚡ Pod CPU | `metricbeat-*` | Line | Avg `kubernetes.pod.cpu.usage.node.pct` |
| 🧠 Memory | `metricbeat-*` | Area | Avg `kubernetes.pod.memory.usage.bytes` |
| 📋 Log Volume | `filebeat-*` | Metric | Count · Last 15 min |

---

<div align="center">

# 🩺 Troubleshooting

</div>

<details>
<summary><b>🔴 &nbsp; kubelet crash-loops on startup</b></summary>
<br>

**Cause:** `SystemdCgroup = false` in containerd config.

```bash
sudo sed -i 's/SystemdCgroup \= false/SystemdCgroup \= true/g' \
  /etc/containerd/config.toml
sudo systemctl restart containerd && sudo systemctl restart kubelet
```
</details>

<details>
<summary><b>🟠 &nbsp; Elasticsearch pod stays Pending</b></summary>
<br>

**Cause:** `vm.max_map_count` too low — run on **ALL nodes.**

```bash
sudo sysctl -w vm.max_map_count=262144
echo "vm.max_map_count=262144" | sudo tee -a /etc/sysctl.conf
kubectl delete pod elasticsearch-master-0 -n logging
```
</details>

<details>
<summary><b>🟡 &nbsp; No logs appearing in Kibana</b></summary>
<br>

**Cause:** Wrong Elasticsearch password in Filebeat config.

```bash
PASSWORD=$(kubectl get secret -n logging elasticsearch-master-credentials \
  -o jsonpath="{.data.password}" | base64 -d)
kubectl logs -n logging -l app=filebeat --tail=50
helm upgrade filebeat elastic/filebeat \
  --namespace logging --values filebeat-config.yaml
```
</details>

<details>
<summary><b>🔵 &nbsp; Nodes stuck NotReady</b></summary>
<br>

**Cause:** Calico CNI not applied.

```bash
kubectl apply -f \
  https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/calico.yaml
kubectl get pods -n kube-system | grep calico
```
</details>

<details>
<summary><b>⚫ &nbsp; ErrImagePull on portfolio pods</b></summary>
<br>

**Cause:** Image not pushed or wrong name.

```bash
docker push <yourusername>/portfolio-site:latest
kubectl rollout restart deployment/portfolio-site
```
</details>

<details>
<summary><b>⚪ &nbsp; kubeadm join token expired</b></summary>
<br>

```bash
kubeadm token create --print-join-command
```
</details>

---

<div align="center">

## ✅ Verified Final State

</div>

```
╔══════════════════════════════════════════════════════════════════╗
║                  CLUSTER — FULLY OPERATIONAL                    ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                  ║
║  ☸️   Kubernetes    v1.30.3   ░░░░░░░░░░░░░░░░░░░  3 / 3 Ready  ║
║  🐳  containerd    latest    ░░░░░░░░░░░░░░░░░░░  SystemdCgroup  ║
║  🌐  Calico        v3.25.0   ░░░░░░░░░░░░░░░░░░░  All pods mesh  ║
║  📦  Pods          running   ░░░░░░░░░░░░░░░░░░░  8 / 8  ✅      ║
║  📋  Log rate      /15 min   ░░░░░░░░░░░░░░░░░░░  1,432+  🔥     ║
║  📈  Metricbeat    interval  ░░░░░░░░░░░░░░░░░░░  10s scrape     ║
║  📊  Kibana        live      ░░░░░░░░░░░░░░░░░░░  v8.5.1  ✅     ║
║                                                                  ║
╚══════════════════════════════════════════════════════════════════╝
```

---

<div align="center">

## 📚 More From This Series

</div>

<div align="center">

| 🗓️ | Article | Highlights |
|:---:|:---|:---|
| Feb 2026 | [**K8s + ELK Stack on Ubuntu 24.04**](https://medium.com/@narengl2001) | This guide — 3-node cluster + full observability |
| Mar 2026 | [**Pod Troubleshooting Field Guide**](https://medium.com/@narengl2001) | Every scheduling & runtime error with kubectl fixes |
| Mar 2026 | [**50 Interview Questions + 24 Scenarios**](https://medium.com/@narengl2001) | Crack any Kubernetes interview |
| Mar 2026 | [**WordPress 5-Server Infra**](https://medium.com/@narengl2001) | Nginx LB · ELK · Prometheus · Grafana |

</div>

---

<div align="center">

## 🤝 Contributing

Fork → Branch → Commit → PR

```bash
git checkout -b feature/your-feature
git commit -m "feat: add your feature"
git push origin feature/your-feature
```

</div>

---

<div align="center">

<!-- ANIMATED FOOTER WAVE -->
<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=6,11,20&height=140&section=footer&text=⭐+Star+this+if+it+helped+you!&fontSize=22&fontColor=a5f3fc&fontAlignY=65&animation=fadeIn" width="100%"/>

<br/>

**Built with ❤️ by [Naren](https://medium.com/@narengl2001)**

*Cloud & DevSecOps Engineer — Building infrastructure, breaking it, fixing it, then writing about it.*

<br/>

[![Medium](https://img.shields.io/badge/Follow_on_Medium-000000?style=for-the-badge&logo=medium&logoColor=white)](https://medium.com/@narengl2001)
[![LinkedIn](https://img.shields.io/badge/Connect_on_LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com)
[![GitHub](https://img.shields.io/badge/More_on_GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com)

<br/>

![Visitor Count](https://visitor-badge.laobi.icu/badge?page_id=narengl2001.k8s-elk-monitoring)

</div>
