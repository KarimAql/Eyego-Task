Eyego DevOps Task - Containerization, Deployment, and Automation  

This task involved containerizing, deploying, and automating a simple Node.js web application using Docker and Kubernetes on AWS. The main goal was to ensure the application runs smoothly in a cloud environment while keeping the setup flexible for potential migration to Google Cloud or Alibaba Cloud.  

### Project Breakdown  

1. **Building the Application**  
I started by creating a basic Node.js application that exposes an API endpoint returning "Hello Eyego." The purpose was to keep the application simple and focus on the DevOps aspects of deployment.  

2. **Containerizing with Docker**  
To make sure the application runs consistently in different environments, I wrote a Dockerfile. It defines the base image, installs dependencies, and sets up the container to run the app. After testing the container locally, I pushed the image to AWS Elastic Container Registry (ECR).  

3. **Deploying on Kubernetes (AWS EKS)**  
The next step was deploying the app on AWS EKS. I wrote Kubernetes YAML files to:  
- Create a Deployment with at least two replicas for high availability  
- Expose the app using a LoadBalancer service to make it accessible externally  

The application is publicly accessible at:  
**http://a888c3a3c73684d0e9177a5a4b15e2d9-601069650.us-east-1.elb.amazonaws.com/**  

4. **Automating with GitHub Actions**  
To streamline the process, I set up a GitHub Actions workflow that:  
- Builds the Docker image  
- Pushes the image to AWS ECR  
- Deploys the latest version to EKS automatically  

This means every time I push a change to the repository, a new deployment happens automatically without manual intervention.  

5. **Preparing for Cloud Migration**  
Since the task also required planning for migration, I documented the steps needed to transition from AWS to Google Cloud Kubernetes Engine (GKE) or Alibaba Cloud Kubernetes. The migration involves:  

**Step 1: Move the Container Image**  
- Push the existing Docker image to Google Container Registry (GCR) or Alibaba Cloud Container Registry (ACR).  
- Update the Kubernetes manifests to use the new container registry.  

**Step 2: Set Up a Kubernetes Cluster**  
- On Google Cloud, create a GKE cluster.  
- On Alibaba Cloud, create an ACK cluster.  

**Step 3: Configure Authentication**  
- Set up kubectl to use the new cluster. (gcloud container clusters get-credentials for GKE, aliyun cs GET-KUBECONFIG for Alibaba Cloud).  

**Step 4: Adjust Kubernetes Configurations**  
- Modify the deployment YAMLs to match the networking setup of the new cloud provider.  
- Replace AWS-specific LoadBalancer settings with alternatives from GKE or Alibaba Cloud.  

**Step 5: Update CI/CD Pipeline**  
- Modify GitHub Actions to push to GCR or ACR instead of AWS ECR.  
- Update deployment steps to work with the new cluster.  


