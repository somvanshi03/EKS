# Prerequsites of EKS:

	# Install eksctl awscli and kubectl utility on windows 
		
		> EKSCTL: https://github.com/eksctl-io/eksctl/releases 
		> AWS CLI: https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
		> kubectl: https://dl.k8s.io/release/v1.31.0/bin/windows/amd64/kubectl.exe 
		
# Create EKS Cluster:
		
		eksctl create cluster --name=eksdemo1 --region=ap-south-1 --zones=ap-south-1a,ap-south-1b --without-nodegroup 

# Get List of Clusters:

		eksctl get clusters
		
		
# Create and Associate IAM OIDC provider for EKS Cluster

		eksctl utils associate-iam-oidc-provider --region ap-south-1 --cluster eksdemo1 --approve		

# Create EKS Node Group:
		
		eksctl create nodegroup --cluster eksdemo1 --region ap-south-1 --name mygroup --node-type t2.medium --nodes 2 --nodes-min 1 --nodes-max 3   --managed --node-volume-size=20 --ssh-access --ssh-public-key=Mumbai-Key-101

# Update kubeconfig file:

		aws eks update-kubeconfig --region ap-south-1 --name eksdemo1
		
# List NodeGroups in a cluster:

		eksctl get nodegroup --cluster=mygroup 

# List Nodes in current kubernetes cluster:

		kubectl get nodes -o wide	
		

# Delete Cluster:

		eksctl delete cluster --name eksdemo1 --region ap-south-1	