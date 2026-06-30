# Setup Guide

This guide explains how to deploy the application on **Amazon EKS** using **AWS Fargate** and the **AWS Load Balancer Controller**.

---

# Prerequisites

Ensure the following tools are installed and configured:

* AWS CLI
* kubectl
* eksctl
* Helm

Configure your AWS credentials:

```bash
aws configure
```

Verify the installation

---

# Step 1: Create an Amazon EKS Cluster

```bash
eksctl create cluster \
  --name 2048-cluster \
  --region ap-south-1 \
  --fargate
```

---

# Step 2: Configure kubectl

```bash
aws eks update-kubeconfig \
  --region ap-south-1 \
  --name 2048-cluster
```

Verify the cluster:

```bash
kubectl get nodes
```

---

# Step 3: Associate the IAM OIDC Provider

```bash
eksctl utils associate-iam-oidc-provider \
  --cluster 2048-cluster \
  --approve
```

---

# Step 4: Create the IAM Policy

Download the policy:

```bash
curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.11.0/docs/install/iam_policy.json
```

Create the policy:

```bash
aws iam create-policy \
  --policy-name AWSLoadBalancerControllerIAMPolicy \
  --policy-document file://iam_policy.json
```

---

# Step 5: Create IAM Role and Service Account

```bash
eksctl create iamserviceaccount \
  --cluster 2048-cluster \
  --namespace kube-system \
  --name aws-load-balancer-controller \
  --role-name AmazonEKSLoadBalancerControllerRole \
  --attach-policy-arn arn:aws:iam::<ACCOUNT_ID>:policy/AWSLoadBalancerControllerIAMPolicy \
  --approve
```

Replace `<ACCOUNT_ID>` with your AWS Account ID.

---

# Step 6: Install the AWS Load Balancer Controller

Add the Helm repository:

```bash
helm repo add eks https://aws.github.io/eks-charts
```

Update the repository:

```bash
helm repo update
```

Install the controller:

```bash
helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
  -n kube-system \
  --set clusterName=2048-cluster \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller \
  --set region=ap-south-1 \
  --set vpcId=<VPC_ID>
```

Replace `<VPC_ID>` with your VPC ID.

Verify the deployment:

```bash
kubectl get deployment -n kube-system aws-load-balancer-controller
```

---

# Step 7: Create a Fargate Profile

```bash
eksctl create fargateprofile \
  --cluster 2048-cluster \
  --region ap-south-1 \
  --name alb-sample-app \
  --namespace game-2048
```

---

# Step 8: Deploy the Application

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/examples/2048/2048_full.yaml
```

---

# Step 9: Verify Resources

Check Pods:

```bash
kubectl get pods -n game-2048
```

Check Services:

```bash
kubectl get svc -n game-2048
```

Check Ingress:

```bash
kubectl get ingress -n game-2048
```

---

# Step 10: Access the Application

Retrieve the ALB DNS name:

```bash
kubectl get ingress -n game-2048
```

Open the generated ALB DNS URL in your browser to access the application.

---

# Clean Up

Delete the EKS cluster and associated resources:

```bash
eksctl delete cluster \
  --name 2048-cluster \
  --region ap-south-1
```
