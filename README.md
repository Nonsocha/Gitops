## Project Guide: Installing & Using Argo CD on AWS EKS
Prerequisites (Must Have)

Before starting, ensure you have:

- An AWS account

- IAM user/role with EKS permissions

- AWS CLI installed and configured

- kubectl installed

- eksctl installed

- GitHub account (for sample app)

### STEP 1: Create an EKS Cluster
1.1 Configure AWS CLI

```
 aws configure
```

### 1.2 Create the EKS Cluster

This creates the control plane + worker nodes.

```
 eksctl create cluster \
  --name argocd-cluster \
  --region us-east-1 \
  --nodegroup-name standard-workers \
  --node-type t3.medium \
  --nodes 2
```

### 1.3 Verify Cluster Access
 
 ```
  kubectl get nodes
```

STEP 2: Install Argo CD on EKS

2.1 Create Argo CD Namespace

```
kubectl create namespace argocd
```

### 2.2 Install Argo CD Manifests

```
kubectl apply -n argocd \
  -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
`````

### 2.3 Verify Argo CD Pods

```
kubectl get pods -n argocd
```

### STEP 3: Access Argo CD UI
3.1 Port Forward Argo CD Server

```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

### 3.2 Open Argo CD UI

In your browser:
``
https://localhost:8080
```
### 3.3 Get Admin Password

```
kubectl get secret argocd-initial-admin-secret \
  -n argocd \
  -o jsonpath="{.data.password}" | base64 --decode
```