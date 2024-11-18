# EKS Cluster with EBS Storage

1. Create IAM policy for EBS
2. attach IAM policy to worker node

## Install EBS CSI driver
	
	aws eks describe-addon-versions --addon-name aws-ebs-csi-driver
	eksctl create addon --name aws-ebs-csi-driver --cluster eksdemo1 --region=ap-south-1  --force
	
## Create resources using kubectl 
	kubectl create -f storageclass.yaml
	kubectl create -f pvc.yaml
	kubectl create -f deployment.yaml
	kubectl get sc
	kubectl get pvc 
	kubectl get deploy 
	kubectl get all
	kubectl log -f deployment ebs-deployment



