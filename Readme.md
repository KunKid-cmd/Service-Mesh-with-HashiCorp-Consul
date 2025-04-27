# Service Mesh with HashiCorp Consul

This project is a demo setup for running Consul on Kubernetes (EKS) using Terraform.
It follows the from Consul crash course by TechWorld with Nana [Consul crash course video](https://www.youtube.com/watch?v=s3I1kKKfjtQ), adapted and customized for personal learning and experimentation.

## 1. Setup

Clone the Repository
```
git clone https://github.com/KunKid-cmd/Service-Mesh-with-HashiCorp-Consul.git
```
original from this[GitLab]https://gitlab.com/twn-youtube/consul-crash-course

## 2. Create a terraform.tfvars

Inside terraform/ directory, create a file named terraform.tfvars with this you aws details:
```
aws_region = "YOUR_AWS_REGION"
aws_access_key_id = "YOUR_AWS_ACCESS_KEY"
aws_secret_access_key = "YOUR_AWS_SECRET_ACCESS_KEY"
```
## 3. Create K8s cluster on AWS EKS

Initialize Terraform
```
cd terraform
terraform init
```
Apply Terraform Configuration
```
terraform apply -var-file terraform.tfvars
```
## 4. Deploy Microservices App on EKS

Configure AWS
```
aws configure
```
Generate Kubeconfig
```
aws eks update-kubeconfig --region <YOUR_AWS_REGION> --name myapp-eks-cluster
```
Verify Access
```
kubectl get nodes
```
change directory to
```
cd kubernetes/
```
apply configuration
```
kubectl apply -f config.yaml
```
Verify pods
```
kubectl get pod
```

