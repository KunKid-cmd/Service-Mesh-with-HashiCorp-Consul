# Service Mesh with HashiCorp Consul

This project is a demo setup for running Consul on Kubernetes (EKS) using Terraform.
It follows the from Consul crash course by TechWorld with Nana [Consul crash course video](https://www.youtube.com/watch?v=s3I1kKKfjtQ), adapted and customized for personal learning and experimentation.

## 1. Setup

Clone the Repository
```
git clone https://github.com/KunKid-cmd/Service-Mesh-with-HashiCorp-Consul.git
```
original from this [GitLab](https://gitlab.com/twn-youtube/consul-crash-course)

## 2. Create a terraform.tfvars

Inside terraform/ directory, create a file named terraform.tfvars with this you aws details:
```
aws_region = "YOUR_AWS_REGION"
aws_access_key_id = "YOUR_AWS_ACCESS_KEY"
aws_secret_access_key = "YOUR_AWS_SECRET_ACCESS_KEY"
```
## 3. Create Kubernetes Cluster on AWS EKS

Initialize and Apply Terraform
```
cd terraform
terraform init
terraform apply -var-file=terraform.tfvars
```
## 4. Deploy Microservices App on EKS

Configure AWS CLI
```
aws configure
```
Generate Kubeconfig for EKS
```
aws eks update-kubeconfig --region <YOUR_AWS_REGION> --name myapp-eks-cluster
```
Verify EKS Cluster
```
kubectl get nodes
```
Deploy Microservices
```
cd kubernetes
kubectl apply -f config.yaml
kubectl get pods
kubectl get svc
```
## 5. Deploy Consul on EKS
Add HashiCorp Helm Repository
```
helm repo add hashicorp https://helm.releases.hashicorp.com
```
Install Consul via Helm
```
helm install eks hashicorp/consul --version 1.0.0 --values consul-values.yaml --set global.datacenter=eks
```
Verify Installation
```
kubectl get pods
kubectl get all
```
## 6. Connect Microservices with Consul
Remove previous services
```
kubectl delete -f config.yaml
```
Apply Consul-aware Configurations
```
kubectl apply -f config-consul.yaml
```
Check Logs
```
kubectl logs <adservice-pod-name>
kubectl logs <adservice-pod-name> -c consul-dataplane
```
## 7. Configure Access Rules via Consul Dashboard
You can create intentions and access control settings through the Consul UI.
## 8. Connect to a Second Kubernetes Cluster
Switch Kubeconfig to the Second Cluster
```
export KUBECONFIG=<path-to-lke-kubeconfig>
chmod 700 <path-to-lke-kubeconfig>
kubectl get nodes
```
Deploy Consul and Microservices on LKE
```
helm install lke hashicorp/consul --version 1.0.0 --values consul-values.yaml --set global.datacenter=lke
kubectl get all
kubectl get crd | grep consul
kubectl get pods
kubectl apply -f config-consul.yaml
kubectl get pods
kubectl get svc
```
## 9. Connect EKS and LKE Clusters (Peer Connections) 
Deploy Mesh Gateway
```
kubectl apply -f consul-mesh-gateway.yaml
kubectl get meshes
```
## 10. Configure Failover Between Clusters
On EKS Terminal
```
kubectl apply -f config-consul.yaml
kubectl apply -f service-resolver.yaml
```
On LKE Terminal
```
kubectl apply -f exported-service.yaml
```
Test Failover
Delete the EKS shippingservice deployment:
```
kubectl delete deployment shippingservice
```
Verify that traffic fails over correctly to the LKE cluster.
