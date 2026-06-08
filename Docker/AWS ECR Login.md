
### After Docker is installed
 * Login to ECR:
```
aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 953146141152.dkr.ecr.ap-south-1.amazonaws.com
```
 * Then:
```hcl
docker build -t prasanth/poultry .
docker tag prasanth/poultry:latest 953146141152.dkr.ecr.ap-south-1.amazonaws.com/prasanth/poultry:latest
docker push 953146141152.dkr.ecr.ap-south-1.amazonaws.com/prasanth/poultry:latest
```
### Verify Image in ECR
```hcl
aws ecr describe-images \
    --repository-name prasanth/poultry \
    --region ap-south-1
```

### Update Kubernetes Deployment
 * Use the ECR image in your deployment:
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
        image: 953146141152.dkr.ecr.ap-south-1.amazonaws.com/prasanth/poultry:latest
        ports:
        - containerPort: 80
```
 * Apply:
```hcl
kubectl apply -f deployment.yaml
```
# 🐳 Docker is install in Amazon Linux

📌 These are the complete commands to build, tag, and push your Docker image to your `AWS ECR repository`.

```hcl
sudo dnf update -y
sudo dnf install docker -y
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -aG docker ec2-user
newgrp docker
docker --version
```

---

## 🐳 After Docker is installed

### 🔐 Login to ECR

```hcl
aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 953146141152.dkr.ecr.ap-south-1.amazonaws.com
```

### 🚀 Then

```hcl
docker build -t prasanth/poultry .
docker tag prasanth/poultry:latest 953146141152.dkr.ecr.ap-south-1.amazonaws.com/prasanth/poultry:latest
docker push 953146141152.dkr.ecr.ap-south-1.amazonaws.com/prasanth/poultry:latest
```

---

## ✅ Verify Image in ECR

```hcl
aws ecr describe-images \
    --repository-name prasanth/poultry \
    --region ap-south-1
```

---

## ☸️ Update Kubernetes Deployment

📦 Use the ECR image in your deployment:

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
        image: 953146141152.dkr.ecr.ap-south-1.amazonaws.com/prasanth/poultry:latest
        ports:
        - containerPort: 80
```

### 🚀 Apply

```hcl
kubectl apply -f deployment.yaml
```

