# 🚀 AWS EKS Project: GoDaddy Domain + Route 53 + Load Balancer + SSL

> End-to-End Production Style Deployment Documentation

---

## 🌐 Architecture

```text
User
  ↓
GoDaddy Domain
  ↓
Route 53 Hosted Zone
  ↓
AWS Load Balancer
  ↓
Kubernetes Service
  ↓
Pods
  ↓
Application
```

---

# 🛒 Step 1: Purchase Domain from GoDaddy

✅ Purchased domain from GoDaddy

Example:

```text
prasanthpoultry.com
```

### Why GoDaddy?

- 💰 Lower registration cost
- ⚡ Easy management
- 🌍 Global DNS support

---

# ☁️ Step 2: Create Route 53 Hosted Zone

1. Open AWS Console
2. Navigate to Route 53
3. Create Hosted Zone
4. Select Public Hosted Zone

Example:

```text
prasanthpoultry.com
```

AWS generates nameservers.

---

# 🔄 Step 3: Update GoDaddy Nameservers

Replace GoDaddy default nameservers with Route 53 nameservers.

```text
ns-123.awsdns-12.com
ns-456.awsdns-34.net
ns-789.awsdns-56.org
ns-111.awsdns-78.co.uk
```

✅ DNS control moved to AWS.

---

# ☸️ Step 4: Create Amazon EKS Cluster

Cluster Details:

```yaml
Cluster Name: eks-cluster
Region: ap-south-1
Version: 1.35
Node Type: t3.medium
```

---

# 🚀 Step 5: Deploy Application

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: poultry-app
spec:
  replicas: 2
```

Deploy:

```bash
kubectl apply -f deployment.yaml
```

---

# ⚖️ Step 6: Create LoadBalancer Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: poultry-app-service
spec:
  type: LoadBalancer
```

Apply:

```bash
kubectl apply -f service.yaml
```

Check:

```bash
kubectl get svc
```

---

# 🌍 Step 7: Connect Domain to Load Balancer

Create Alias A Record in Route 53.

```text
prasanthpoultry.com
      ↓
AWS Load Balancer
```

---

# 🔒 Step 8: Request SSL Certificate

AWS Certificate Manager

Domains:

```text
prasanthpoultry.com
*.prasanthpoultry.com
```

Validation Method:

✅ DNS Validation

---

# ✅ Step 9: Validate Certificate

Create ACM-generated CNAME records in Route 53.

Certificate Status:

```text
Pending Validation
        ↓
      Issued
```

---

# 🔐 Step 10: Configure HTTPS

Attach ACM Certificate to Ingress/ALB.

```yaml
alb.ingress.kubernetes.io/certificate-arn: <certificate-arn>
```

---

# 🧪 Step 11: Verification

```bash
nslookup prasanthpoultry.com
```

Open:

```text
https://prasanthpoultry.com
```

Expected:

- ✅ Domain Working
- ✅ HTTPS Enabled
- ✅ SSL Valid
- ✅ Load Balancer Connected
- ✅ Application Accessible

---

# 🛠 AWS Services Used

| Service | Purpose |
|----------|----------|
| EKS | Kubernetes |
| EC2 | Worker Nodes |
| ELB | Traffic Routing |
| Route 53 | DNS |
| ACM | SSL |
| ECR | Docker Images |
| IAM | Permissions |

---

# 🎤 Interview Explanation

> I purchased the domain from GoDaddy because Route 53 registration was more expensive. I created a Route 53 Hosted Zone and updated the GoDaddy nameservers to Route 53 nameservers. After deploying my application on Amazon EKS, I exposed it using a Kubernetes LoadBalancer Service which created an AWS Load Balancer. I then created an Alias A Record in Route 53 pointing the domain to the Load Balancer. For HTTPS, I requested an SSL certificate from AWS Certificate Manager, validated it through DNS, and attached it to the Load Balancer using Kubernetes Ingress. This allowed secure access to the application through a custom domain.
