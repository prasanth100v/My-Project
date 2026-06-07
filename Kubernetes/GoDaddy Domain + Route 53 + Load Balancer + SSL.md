# 🚀 AWS EKS Project: GoDaddy Domain + Route 53 + Load Balancer + SSL
 * End-to-End Production Style Deployment Documentation

## 🌐 Architecture
```hcl
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

# 🛒 Step 1: Purchase Domain from GoDaddy
 * ✅ Purchased domain from GoDaddy
 * Example:
     * `prasanthpoultry.in`
 * Verified domain ownership through GoDaddy dashboard.

### Why GoDaddy?
  - 💰 Lower registration cost
  - ⚡ Easy management
  - 🌍 Global DNS support

# Step 2: Create Route 53 Hosted Zone
 * Open AWS Console.
 * Navigate to Route 53.
 * Click "Hosted Zones".
 * Create Hosted Zone.
     * Enter domain name: `prasanthpoultry.in`
 * Select:
     * Public Hosted Zone
     * Create Hosted Zone.
 * AWS automatically generated Name Servers:
 * Example: 
```hcl
ns-123.awsdns-12.com
ns-456.awsdns-34.net
ns-789.awsdns-56.org
ns-111.awsdns-78.co.uk
```
 * Copy Route 53 Name Servers
 * After creation, you will see an NS record with 4 nameservers Copy all 4...

Step 3: Update GoDaddy Nameservers
 * Replace GoDaddy `default nameservers` with `Route 53 nameservers`.
 * Login to GoDaddy.
 * Open Domain Management.
 * Select the purchased domain.
 * Open DNS Settings.
     * Find: Nameservers
     * Click: Change Nameservers
     * Choose: I'll use my own nameservers
 * Paste the 4 Route 53 nameservers.
```hcl
ns-123.awsdns-12.com
ns-456.awsdns-34.net
ns-789.awsdns-56.org
ns-111.awsdns-78.co.uk
```
 * Save changes.
 * Purpose:
    * ✅ Now DNS control moved from GoDaddy to AWS Route 53.
 * DNS propagation may take:
    * Few minutes
    * Up to 48 hours


# 🚀 Step 5: Deploy Application
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: poultry-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: poultry-app
  template:
    metadata:
      labels:
        app: poultry-app
    spec:
      containers:
      - name: poultry-app
        image: prasanth100v/prasanth-poultry:latest
        imagePullPolicy: Always
        ports:
        - containerPort: 80
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
  selector:
    app: poultry-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
    nodePort: 30080
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
 * Create Alias A Record in Route 53.
 * Open :
    * Route 53 → Hosted Zone → prasanthpoultry.in → Create Record
 * Configure:
    * Record Type: A
    * Alias: ON
    * Route Traffic To: Alias to `Application Load Balancer`
    * Select your ALB:
        * `ac1ebfe6952b14c53b8fef4809bac5e1-1241331641.ap-south-1.elb.amazonaws.com`
   * Create Record.
 * Verify
    * After `5–30 minutes`:
      * `http://prasanthpoultry.in` → should open your application.

# 🔒 Step 8: Request SSL Certificate
 * AWS Certificate Manager
 * click Domains:
```hcl
prasanthpoultry.com
*.prasanthpoultry.com
```
 * Validation Method:
 * ✅ DNS Validation

---

## Update service.yaml
```yaml
apiVersion: v1
kind: Service
metadata:
  name: poultry-app-service
  annotations:
    service.beta.kubernetes.io/aws-load-balancer-ssl-cert: arn:aws:acm:ap-south-1:953146141152:certificate/f93de687-3b6e-4953-9690-2e4cbba3ff93
    service.beta.kubernetes.io/aws-load-balancer-backend-protocol: http
    service.beta.kubernetes.io/aws-load-balancer-ssl-ports: "443"
spec:
  type: LoadBalancer
  selector:
    app: poultry-app
  ports:
  - name: http
    protocol: TCP
    port: 80
    targetPort: 80

  - name: https
    protocol: TCP
    port: 443
    targetPort: 80
```
Apply the Changes
```hcl
kubectl apply -f service.yaml
```

## Verify in AWS
 * Go to: AWS Console → EC2 → Load Balancers → Your CLB → Listeners
 * You should see:

| Load Balancer Port | Instance Port |
| ------------------ | ------------- |
| 80                 | 80            |
| 443                | 80            |


---

# 🛠 AWS Services Used
| 🧩 **AWS Service**                 | 🎯 **Purpose**         | 📖 **Description**                               | 💡 **Example Usage**              |
| ---------------------------------- | ---------------------- | ------------------------------------------------ | --------------------------------- |
| ☸️ Amazon EKS                         | Kubernetes Platform    | Managed Kubernetes control plane                 | Run containerized applications    |
| 🖥️ Amazon EC2                         | Worker Nodes           | Virtual machines that run Kubernetes pods        | EKS node groups                   |
| ⚖️ Elastic Load Balancing             | Traffic Routing        | Distributes incoming traffic across applications | Application Load Balancer (ALB)   |
| 🌐 Amazon Route 53                    | DNS Management         | Maps domain names to AWS resources               | `app.example.com` → Load Balancer |
| 🔐 AWS Certificate Manager              | SSL/TLS Certificates   | Manages HTTPS certificates                       | Secure website traffic            |
| 🐳 Amazon ECR /Docker               | Docker Image Storage   | Stores and manages container images              | Push/pull Docker images           |
| 🌍 AWS Identity and Access Management   | Permissions & Security | Controls access to AWS resources                 | EKS node roles, service accounts  |

---

# 🎤 Interview Explanation

> I purchased the domain from GoDaddy because Route 53 registration was more expensive. I created a Route 53 Hosted Zone and updated the GoDaddy nameservers to Route 53 nameservers. After deploying my application on Amazon EKS, I exposed it using a Kubernetes LoadBalancer Service which created an AWS Load Balancer. I then created an Alias A Record in Route 53 pointing the domain to the Load Balancer. For HTTPS, I requested an SSL certificate from AWS Certificate Manager, validated it through DNS, and attached it to the Load Balancer using Kubernetes Ingress / service. This allowed secure access to the application through a custom domain.
