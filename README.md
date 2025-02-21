## Eyego DevOps Task - Containerization, Deployment, and Automation

As part of this task, I implemented a complete workflow for containerizing, deploying, and automating a simple Node.js web application using Docker and Kubernetes on AWS. The objective was to ensure that the application runs reliably in a cloud environment while maintaining modularity for potential migration to Google Cloud or Alibaba Cloud.

---

## Project Breakdown

### 1. Developing the Application
I started by creating a basic Node.js application that exposes an API endpoint returning "Hello Eyego." The goal was to keep the application lightweight and functional while focusing on the DevOps aspects of deployment. The application consists of a simple `server.js` file that handles HTTP requests and responses. The necessary dependencies were managed through a `package.json` file.

### 2. Containerizing the Application
To ensure consistency across different environments, I wrote a Dockerfile to containerize the Node.js application. The Dockerfile:
- Specifies the base image as `node:latest` to provide an optimized runtime environment.
- Copies the application source code into the container.
- Installs required dependencies using `npm install`.
- Defines `CMD` to run the application.

Once the Dockerfile was ready, I built the image and tested it locally using Docker. After verifying its functionality, I pushed the built image to AWS Elastic Container Registry (ECR) for use in the Kubernetes deployment.

### 3. Setting Up Kubernetes on AWS
Since the application needed to be deployed on AWS Kubernetes (EKS), I wrote Kubernetes manifests to define the deployment and service.

- **Deployment.yaml:** Defines the deployment, including replica count (at least two replicas) and the container image reference from AWS ECR.
- **Service.yaml:** Configures a LoadBalancer to expose the application externally.

After ensuring the configurations were correct, I applied them using `kubectl apply -f` commands. This ensured that the application was deployed and accessible.

**Public URL:** http://a888c3a3c73684d0e9177a5a4b15e2d9-601069650.us-east-1.elb.amazonaws.com/

### 4. Automating with GitHub Actions
To streamline the deployment process, I created a GitHub Actions workflow that automates the build and deployment process. The workflow performs the following tasks:

- **Step 1: Trigger the Workflow**
  - The pipeline is triggered on every push or merge to the main branch.

- **Step 2: Build the Docker Image**
  - The workflow checks out the repository and builds the Docker image using the Dockerfile.

- **Step 3: Authenticate with AWS ECR**
  - Uses AWS credentials to authenticate with ECR.

- **Step 4: Push the Docker Image to AWS ECR**
  - The built image is pushed to AWS ECR, ensuring that the latest version is available for deployment.

- **Step 5: Deploy the Application to AWS EKS**
  - Uses `kubectl apply -f` to deploy the updated Kubernetes configurations.

This setup ensures that any change pushed to the repository automatically updates the running application without manual intervention.

---

## Migration to Google Cloud Kubernetes Engine (GKE) or Alibaba Cloud Kubernetes (ACK)

Since modularity was a key requirement, I documented the steps needed to migrate this setup from AWS to Google Cloud Kubernetes Engine (GKE) or Alibaba Cloud Kubernetes (ACK).

### Step 1: Container Registry Migration
- Push the existing Docker image to Google Container Registry (GCR) or Alibaba Cloud Container Registry (ACR).
- Update Kubernetes manifests (`deployment.yaml`) to reference the new container image location.

### Step 2: Kubernetes Cluster Setup
- On **Google Cloud**, create a GKE cluster using:
  ```
  gcloud container clusters create eyego-cluster --num-nodes=2
  ```
- On **Alibaba Cloud**, create an ACK cluster using:
  ```
  aliyun cs CREATE-KUBERNETES-CLUSTER --region us-east-1 --name eyego-cluster
  ```

### Step 3: Authentication and Access
- Configure `kubectl` to use the new cluster:
  - **Google Cloud:**
    ```
    gcloud container clusters get-credentials eyego-cluster
    ```
  - **Alibaba Cloud:**
    ```
    aliyun cs GET-KUBECONFIG --region us-east-1 --name eyego-cluster
    ```

### Step 4: Deployment Adjustments
- Modify the Kubernetes deployment YAML files to:
  - Update the container image URL.
  - Adjust networking configurations based on the cloud provider.
  - Replace AWS-specific LoadBalancer settings with appropriate configurations for GKE or ACK.

### Step 5: CI/CD Integration
- Modify GitHub Actions to authenticate with GCR or ACR instead of AWS ECR.
- Update deployment scripts to apply changes to the new Kubernetes cluster.


