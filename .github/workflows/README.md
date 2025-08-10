# Patient Web Interface - DevOps Project

This repository contains a **Patient Web Interface** built with Python Flask, along with complete DevOps infrastructure using Docker, Terraform, Kubernetes, and GitHub Actions for automation and CI/CD.

---

## 📦 Project Components

* **Flask Web Application**: A simple frontend + backend for patient registration and record search.
* **Dockerfile**: Contains steps to build the application into a container image.
* **GitHub Actions**: Automates Docker build and image push to Docker Hub.
* **Terraform Scripts**: Provision AWS infrastructure like EC2 instance, VPC, Subnets, and Security Groups.
* **Kubernetes Manifests**: Define how the app is deployed and exposed inside Minikube.

---

## ⚙️ Setup and Deployment

### 🔧 Prerequisites

Before proceeding, make sure you have the following installed:

1. **Git** - to clone and manage repositories.
2. **Terraform** - for AWS infrastructure provisioning.
3. **AWS CLI** - with your credentials configured.
4. **Docker** - for containerizing and running the app.
5. **Docker Hub Account** - to push the built image.
6. **kubectl** - CLI tool to interact with Kubernetes.
7. **Minikube** - for running a local Kubernetes cluster.

---

### 1. Clone the Repository

```bash
git clone https://github.com/Rewaa155/Patient-Web-interface.git
cd Patient-Web-interface
```

---

### 2. Dockerization

Build and test your Docker image locally, or let GitHub Actions handle it on push.

**Manual Build:**

```bash
docker build -t rewaa155/patient-web-interface .
docker push rewaa155/patient-web-interface:latest
```

---

### 3. Provision AWS Infrastructure with Terraform

Navigate to the `terraform/` directory:

```bash
cd terraform
cp terraform.tfvars.example terraform.tfvars
# Then update terraform.tfvars with your values (Key Pair, AMI, etc.)
```

**Initialize and Apply:**

```bash
terraform init
terraform plan
terraform apply
```

This sets up an EC2 instance with required networking.

---

### 4. Setup Minikube on the EC2 Instance

SSH into the EC2 instance:

```bash
ssh -i /path/to/key.pem ubuntu@<EC2_PUBLIC_IP>
```

**Install Docker & Minikube Tools:**

```bash
# Docker
sudo apt-get update && sudo apt-get install -y docker.io
sudo systemctl start docker
sudo usermod -aG docker ubuntu
newgrp docker

# kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# Minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# Start Minikube
minikube start --driver=docker
```

---

### 5. Deploy to Kubernetes

Copy deployment files to the EC2 instance:

```bash
scp -i /path/to/key.pem -r kubernetes ubuntu@<EC2_PUBLIC_IP>:~/
scp -i /path/to/key.pem scripts/deploy-to-minikube.sh ubuntu@<EC2_PUBLIC_IP>:~/
```

Run the script on EC2:

```bash
ssh -i /path/to/key.pem ubuntu@<EC2_PUBLIC_IP>
chmod +x deploy-to-minikube.sh
./deploy-to-minikube.sh
```

---

### 6. Access the Application

Port-forward the service:

```bash
kubectl port-forward service/patient-web-interface-service 8080:80
```

Now open your browser and go to:

```
http://<EC2_PUBLIC_IP>:8080
```

---

## 🚀 CI/CD Pipeline (GitHub Actions)

Located at `.github/workflows/ci-cd.yml`, the pipeline will:

* Clone the repository
* Log in to Docker Hub using GitHub secrets
* Build the Docker image
* Push the image to Docker Hub (`rewaa155/patient-web-interface:latest`)

> ⚠️ Be sure to set `DOCKER_USERNAME` and `DOCKER_PASSWORD` secrets in your GitHub repository settings.

---

## 📸 Screenshots

![App Running](images/Screenshot1.png)

---

## 📚 Technologies Used

* **Python / Flask** — Web application
* **Terraform** — Infrastructure as Code
* **Docker** — Containerization
* **GitHub Actions** — CI/CD Pipeline
* **Kubernetes** — Container orchestration via Minikube






   
