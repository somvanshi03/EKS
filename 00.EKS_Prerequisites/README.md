# EKS 

## Prerequisites

```bash
# Install eksctl

curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_Linux_amd64.tar.gz" | tar xz -C /tmp

sudo mv /tmp/eksctl /usr/local/bin


# AWS CLI
sudo apt install awscli -y

# kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin

```

## EKS Setup

```bash
# Create EKS

eksctl create cluster --name my-eks-cluster --region ap-south-1 --version 1.29 --nodegroup-name my-node-group --node-type t3.medium --nodes 2 --nodes-min 2 --nodes-max 4 --managed
  

# Step: Enable IAM OIDC (Important)

eksctl utils associate-iam-oidc-provider --region ap-south-1 --cluster my-eks-cluster --approve
  
# Configure kubectl

aws eks update-kubeconfig --region ap-south-1 --name my-eks-cluster
  
# Verify Cluster

kubectl get nodes
kubectl get pods -A 

# List clusters

eksctl get cluster

# Delete cluster

eksctl delete cluster --name my-eks-cluster --region ap-south-1

```
