# Patient Web Interface - DevOps Project
This repository contains a Patient Web Interface application and the necessary infrastructure as code (IaC), CI/CD pipelines, and Kubernetes deployment configurations to deploy and manage it.

Project Components
Patient Web Interface Application: A Python Flask application.
Dockerfile: Defines the Docker image for the application.
GitHub Actions: CI/CD pipeline for building Docker images and pushing to Docker Hub.
Terraform: Infrastructure as Code for provisioning AWS resources (EC2, VPC, Subnets).
Kubernetes Manifests: Deployment and Service definitions for deploying the application on a Kubernetes cluster (Minikube).
Setup and Deployment
Prerequisites
Before you begin, ensure you have the following installed:

Git
Docker Desktop (or Docker Engine)
Docker Hub account
AWS CLI configured with appropriate credentials
Terraform
kubectl
Minikube
1. Clone the Repository
git clone https://github.com/AbdallahBagato/Patient-Web-interface.git
cd Patient-Web-interface
2. Dockerization
The project/Dockerfile defines how to build the application's Docker image. The GitHub Actions workflow automatically builds and pushes this image to Docker Hub.

3. AWS Infrastructure with Terraform
Navigate to the terraform directory:

cd terraform
Copy the example variables file and update it with your AWS key pair name and desired AMI ID:

cp terraform.tfvars.example terraform.tfvars
# Edit terraform.tfvars with your AWS specific values
Initialize Terraform, plan, and apply the infrastructure:

terraform init
terraform plan
terraform apply
This will provision an EC2 instance, VPC, subnets, and security groups in your AWS account.

4. Minikube Setup on EC2
Once the EC2 instance is provisioned, SSH into it. You will need to install Minikube and kubectl on the EC2 instance. Here are the general steps:

# SSH into your EC2 instance
ssh -i /path/to/your/key.pem ubuntu@<EC2_PUBLIC_IP>

# Install Docker (if not already installed)
sudo apt-get update
sudo apt-get install -y docker.io
sudo systemctl start docker
sudo usermod -aG docker ubuntu
newgrp docker

# Install kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# Install Minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# Start Minikube
minikube start --driver=docker
5. Kubernetes Deployment
After Minikube is running on your EC2 instance, copy the kubernetes directory and scripts/deploy-to-minikube.sh to your EC2 instance. Then, execute the deployment script:

# On your local machine, copy files to EC2
scp -i /path/to/your/key.pem -r kubernetes ubuntu@<EC2_PUBLIC_IP>:~/
scp -i /path/to/your/key.pem scripts/deploy-to-minikube.sh ubuntu@<EC2_PUBLIC_IP>:~/

# SSH back into your EC2 instance
ssh -i /path/to/your/key.pem ubuntu@<EC2_PUBLIC_IP>

# Make the script executable and run it
chmod +x deploy-to-minikube.sh
./deploy-to-minikube.sh
This script will apply the Kubernetes deployment and service manifests to your Minikube cluster.

6. Accessing the Application
To access the application, use kubectl port-forward from your EC2 instance:

kubectl port-forward service/patient-web-interface-service 8080:80
Then, from your local machine, you can access the application by navigating to http://<EC2_PUBLIC_IP>:8080 in your web browser.

CI/CD Pipeline (GitHub Actions)
The .github/workflows/ci-cd.yml file defines the CI/CD pipeline. On every push to the main branch, the workflow will:

Checkout the code.
Log in to Docker Hub using DOCKER_USERNAME and DOCKER_PASSWORD secrets.
Build the Docker image for the application.
Push the storm00/patient-web-interface:latest image to Docker Hub.
Note: Ensure you have configured DOCKER_USERNAME and DOCKER_PASSWORD as secrets in your GitHub repository settings.
