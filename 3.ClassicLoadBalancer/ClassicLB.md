# Classic Load balancer:


## Create Deployments and Service
	
	kubectl apply -f my-app-deployment.yaml
	kubectl apply -f my-app-service.yaml

## Get Service and Load Balancer details

	kubectl get svc my-app-service
	kubectl get deployment my-app-deployment

	