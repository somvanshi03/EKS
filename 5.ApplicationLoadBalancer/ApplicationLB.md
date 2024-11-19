# Application LoadBalancer

## Create Service and Deployment

	kubectl apply -f deployment.yaml
	kubectl apply -f service.yaml
	kubectl apply -f ingressLB.yaml

## Get Service and Deployment details

	kubectl get ingress my-app-ingress
	kubectl get svc 
	kubectl get deployment
	
## (Optional) Configure SSL/TLS for HTTPS
	
	annotations:
		alb.ingress.kubernetes.io/ssl-cert: <your-ssl-certificate-arn>
		alb.ingress.kubernetes.io/listen-ports: '[{"HTTP":80}, {"HTTPS":443}]'
