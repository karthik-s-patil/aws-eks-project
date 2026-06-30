# aws-eks-fargate-alb-project

## Overview

This project demonstrates the deployment of a containerized application on **Amazon EKS using AWS Fargate** and exposing it to the internet using an **Application Load Balancer (ALB)** via the **AWS Load Balancer Controller**.

The architecture follows AWS and Kubernetes best practices using serverless compute, secure IAM integration, and Ingress-based traffic routing.

---

## Architecture Components

### Compute

* Amazon EKS Cluster
* AWS Fargate (Serverless Pods)
* Kubernetes Pods (2048 Application)

### Kubernetes Resources

* Namespace
* Deployment
* Service (ClusterIP)
* Ingress

### Networking & Load Balancing

* AWS Application Load Balancer (ALB)
* Target Groups
* AWS Load Balancer Controller

### Security & Access

* IAM Roles
* IAM Policies
* OIDC Provider
* IAM Roles for Service Accounts (IRSA)

---

## AWS Services Used

| Service                      | Purpose                       |
| ---------------------------- | ----------------------------- |
| Amazon EKS                   | Kubernetes Cluster Management |
| AWS Fargate                  | Serverless Compute for Pods   |
| IAM                          | Access Control                |
| OIDC                         | Identity Federation           |
| IRSA                         | Secure Pod-level AWS Access   |
| ALB                          | External Traffic Routing      |
| EC2 Networking (VPC default) | Cluster Networking            |

---

## Key Learning Outcomes

* Amazon EKS cluster deployment using eksctl
* AWS Fargate-based serverless Kubernetes scheduling
* Kubernetes networking (Service & Ingress)
* AWS Load Balancer Controller integration
* IAM Roles for Service Accounts (IRSA)
* OIDC-based secure authentication between AWS and Kubernetes
* End-to-end traffic flow from ALB to Kubernetes Pods

---

## Author

**Karthik S Patil**
BCA Graduate | Aspiring Cloud & DevOps Engineer

GitHub: https://github.com/karthik-s-patil
LinkedIn: https://www.linkedin.com/in/karthikspatil2004
