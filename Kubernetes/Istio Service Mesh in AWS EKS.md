# 🚀 Istio Service Mesh in AWS EKS
### 📌 What is Istio?
 * 🔹 Istio is an open-source `Service Mesh` that manages communication between microservices running in Kubernetes.
 * 🔹 It provides:
     * ✅ Traffic Management
     * ⚖️ Load Balancing
     * 🔍 Service Discovery
     * 🔒 Security (`mTLS`)
     * 📊 Observability & Monitoring
     * 🚀 Canary Deployments
     * 🔄 Blue-Green Deployments
     * 💥 Fault Injection & Circuit Breaking

## 🏗️ Architecture
```hcl
                    🌐 Internet
                        │
                        ▼
              ⚖️ AWS Load Balancer
                        │
                        ▼
                 🚪 Istio Gateway
                        │
                        ▼
                🔀 Virtual Service
                        │
        ┌───────────────┼───────────────┐
        ▼               ▼               ▼
    📦 Service A    📦 Service B    📦 Service C
        │               │               │
    🔹 Envoy Proxy  🔹 Envoy Proxy  🔹 Envoy Proxy
        │               │               │
        └───────────────┼───────────────┘
                        │
                  🎛️ Istio Control Plane
                       (istiod)
```

---

## 🔹 Step 1: Install Istio CLI
⬇️ Download Istio:
```hcl
curl -L https://istio.io/downloadIstio | sh -
```

📦 Move binary:
```hcl
sudo mv istio-*/bin/istioctl /usr/local/bin/
```

✅ Verify:
```hcl
istioctl version
```

---

## 🔹 Step 2: Install Istio
🚀 Install Istio:
```hcl
istioctl install --set profile=demo -y
```

🔍 Check:
```hcl
kubectl get pods -n istio-system
```

Expected:
```hcl
istiod
istio-ingressgateway
```

---

## 🔹 Step 3: Enable Sidecar Injection
🏷️ Label Namespace:
```hcl
kubectl label namespace default istio-injection=enabled
```

🔍 Verify:
```hcl
kubectl get ns --show-labels
```

Output:
```hcl
default   istio-injection=enabled
```

---

## 📊 Step 4: Install Istio Monitoring
📥 Install addons:
```hcl
kubectl apply -f samples/addons
```

🔍 Check:
```hcl
kubectl get pods -n istio-system
```

| Tool          | Purpose           |
| ------------- | ----------------- |
| 📈 Prometheus | Metrics           |
| 📊 Grafana    | Dashboards        |
| 🕸️ Kiali     | Service Mesh View |
| 🔍 Jaeger     | Tracing           |

---

## 🌐 Access Kiali and Jaeger through Load Balancer
### 🕸️ Kiali
```hcl
kubectl patch svc kiali -n istio-system -p '{"spec":{"type":"LoadBalancer"}}'
```

### 🔍 Jaeger
```hcl
kubectl patch svc tracing -n istio-system -p '{"spec":{"type":"LoadBalancer"}}'
```

### 🔍 Check
```hcl
kubectl get svc -n istio-system
```
 * ⏳ Wait 1–3 minutes and AWS will create separate ELBs for Kiali and Jaeger.


### ✅ Your Jaeger LoadBalancer is created successfully
```hcl
aaf41b9f7bbc3404eb4e87ed493f516d-346224214.ap-south-1.elb.amazonaws.com
```

### ✅ Your Kiali LoadBalancer is created successfully
```hcl
http://aaf41b9f7bbc3404eb4e87ed493f516d-346224214.ap-south-1.elb.amazonaws.com:20001
```
 * ⚠️ **Common Issue**
 * 🔸 Many AWS Classic Load Balancers created by Kubernetes only listen on: `Port 80`, `Port 443`.
 * 🔸 While Kiali is serving on: `Port 20001`
      * 🔸 In that case, add `20001` port on LoadBalancer.

---

## 📈 Benefits of Istio in EKS
| 🧩 **Feature**           | 🎯 **Benefit**                  | 📖 **Description**                                               | 💡 **Real-World Use Case**                    |
| ------------------------- | ------------------------------- | ---------------------------------------------------------------- | ---------------------------------------------- |
| 🕸️ **Service Mesh**      | Manages Microservices           | Provides a dedicated layer for `service-to-service communication` | Large microservices architectures              |
| 🔒 **mTLS**               | Secure Service Communication    | Encrypts and authenticates traffic between services             |` Zero Trust networking    `                     |
| 🚦 **Traffic Routing**    | `Canary` & `Blue-Green Deployments` | Controls how traffic is distributed `across application versions` | Safe production releases                  |
| 📊 **Observability**      | Metrics & Tracing               | Collects metrics, logs, and distributed traces                  | Monitoring with `Prometheus`, `Grafana`, and `Jaeger` |
| ⚖️ **Load Balancing**     | Smart Traffic Distribution      | Supports `ROUND_ROBIN`, `LEAST_CONN`, and other `algorithms  `  | Improved performance and availability           |
| 🛡️ **Circuit Breaking**  | Prevent Cascading Failures       |  `Stops traffic to unhealthy services  `                        | Prevents system-wide outages                     |
| 💥 **Fault Injection**    | Test Failure Scenarios          | Simulates `delays` and `errors`                                 | Chaos engineering and resilience testing        |
| 🔐 **Policy Enforcement** | Security Controls               | Enforces `authentication` and `authorization policies `         | Service access control                          |
| 🔁 **Retries & Timeouts** | Improved Reliability            | Automatically `retries failures` and `limits request duration`  | Handles transient network issues                |
| 🌍 **Traffic Splitting**  | Progressive Delivery            | Routes traffic by `percentage`                                  | A/B testing and canary releases                 |

---

## 🎯 Production Architecture
```hcl
🌐 Internet
    │
    ▼
📍 Route53
    │
    ▼
⚖️ AWS Load Balancer
    │
    ▼
🚪 Istio Gateway
    │
    ▼
🔀 Virtual Service
    │
    ▼
🔗 Kubernetes Service
    │
    ▼
📦 Pods + Envoy Sidecars
    │
    ▼
🎛️ Istiod Control Plane
    │
    ▼
📊 Prometheus + Grafana + Kiali
```
