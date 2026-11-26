# AWS ECS Fargate – Microservices Architecture

## 1. Project Overview

This project implements a **microservices-based application architecture using Amazon ECS with AWS Fargate**. The infrastructure is deployed inside a custom Amazon VPC, with public and private subnets distributed across multiple Availability Zones.

Each microservice runs independently as an ECS Fargate task, allowing services to be deployed, scaled, and maintained independently.

The architecture uses AWS services for container orchestration, networking, security, CI/CD automation, container image management, and monitoring.



## 2. AWS Services Used

| AWS Service | Purpose |
|---|---|
| Amazon VPC | Provides an isolated network environment. |
| Public Subnets | Host public-facing infrastructure and NAT Gateways. |
| Private Subnets | Host ECS Fargate microservices. |
| Availability Zones | Distribute workloads across multiple zones. |
| Amazon ECS | Manages and orchestrates containerized applications. |
| AWS Fargate | Runs containers without managing EC2 instances. |
| Amazon ECR | Stores Docker images of microservices. |
| Application Load Balancer | Distributes incoming application traffic. |
| Amazon Route 53 | Manages domain names and DNS routing. |
| AWS WAF | Protects the application against common web attacks. |
| AWS Certificate Manager | Provides SSL/TLS certificates for HTTPS. |
| NAT Gateway | Provides outbound internet connectivity for private subnet workloads. |
| AWS IAM | Manages permissions and access to AWS resources. |
| AWS Secrets Manager | Manages application secrets and credentials. |
| AWS CodePipeline | Automates the CI/CD workflow. |
| AWS CodeBuild | Builds application code and Docker images. |
| AWS CodeDeploy | Handles application deployment. |
| Amazon S3 | Stores CI/CD pipeline artifacts. |
| Amazon CloudWatch | Collects application logs and monitoring information. |

## 3. Microservices Architecture

The application consists of four independent microservices deployed using ECS Fargate.

### 3.1 User Service
- Handles user-related application operations.
- Runs inside an ECS Fargate task.
- Communicates with other microservices through API calls.

### 3.2 Profile API Service
- Handles user profile-related operations.
- Runs independently inside an ECS Fargate task.
- Receives API requests from other microservices.

### 3.3 Admin Service
- Handles administrative application operations.
- Runs inside an ECS Fargate task.
- Communicates with other microservices when required.

### 3.4 Property API Service
- Handles property-related application operations.
- Runs independently inside an ECS Fargate task.
- Supports communication with other application services.

Each microservice has its own container image stored in Amazon ECR and runs as an ECS Fargate task.

## 4. Network Architecture

The infrastructure is deployed inside an Amazon VPC.

### Public Subnets
Public subnets contain:
- Application Load Balancer.
- NAT Gateways.

The public-facing Application Load Balancer receives incoming HTTPS requests and forwards traffic to the application services.

NAT Gateways provide outbound internet connectivity for workloads running in private subnets.

### Private Subnets
Private subnets contain the ECS Fargate microservices:
- User Service
- Profile API Service
- Admin Service
- Property API Service

The application containers run without requiring public IP addresses.

### Availability Zones

The architecture distributes workloads across multiple Availability Zones to improve availability and fault tolerance.

Security groups control communication between the application components and AWS resources.

## 5. Application Request Flow

The incoming application request follows this sequence:

1. Users access the application through a domain name.
2. Amazon Route 53 resolves the domain to the application endpoint.
3. AWS WAF inspects incoming requests and applies configured security rules.
4. The Application Load Balancer receives HTTPS traffic.
5. AWS Certificate Manager provides the SSL/TLS certificate for secure communication.
6. The ALB forwards requests to the appropriate application service.
7. ECS Fargate runs the microservices inside private subnets.
8. Microservices communicate with other services through internal API calls.
9. Responses are returned to the user through the application entry point.

## 6. Container Image Management

Amazon ECR is used as the centralized container image registry.

The workflow is:

1. Develop the application.
2. Create a Dockerfile for each microservice.
3. Build Docker images.
4. Push the images to Amazon ECR.
5. ECS Fargate pulls the required images from ECR.
6. ECS runs the containers using the configured task definitions.

ECR provides centralized storage and version management for application container images.

## 7. CI/CD Pipeline

The architecture uses AWS CodePipeline, CodeBuild, and CodeDeploy to automate application deployment.

### CI/CD Workflow

1. Developers push application changes to GitHub.
2. GitHub changes trigger AWS CodePipeline.
3. CodePipeline starts the CI/CD workflow.
4. CodeBuild builds the application and Docker images.
5. Docker images are pushed to Amazon ECR.
6. Deployment artifacts are stored in Amazon S3.
7. CodeDeploy handles the deployment process.
8. Updated application versions are deployed to ECS Fargate.

IAM roles provide the required permissions for the CI/CD services to access AWS resources.

## 8. Security Implementation

Security is implemented at multiple levels.

- **Network Security:** ECS tasks run inside private subnets.
- **Security Groups:** Control inbound and outbound traffic.
- **IAM Roles:** Provide controlled access to AWS services.
- **AWS WAF:** Protects the public-facing application.
- **HTTPS:** Secures communication using SSL/TLS certificates.
- **Secrets Management:** AWS Secrets Manager manages sensitive application credentials.
- **Private Container Workloads:** ECS tasks do not require direct public internet exposure.

## 9. Monitoring and Logging

Amazon CloudWatch is integrated with ECS Fargate for application monitoring and logging.

CloudWatch can be used to monitor:

- ECS task status.
- Container logs.
- Application errors.
- CPU utilization.
- Memory utilization.
- Application performance.

This helps identify application failures and troubleshoot production issues.

## 10. Key Architecture Benefits

- **Serverless Container Management:** AWS Fargate eliminates the need to manage EC2 worker instances.
- **Microservices Architecture:** Each application service operates independently.
- **High Availability:** Workloads are distributed across multiple Availability Zones.
- **Scalability:** Individual ECS services can scale independently.
- **Security:** Private subnets, IAM roles, security groups, WAF, and Secrets Manager provide multiple security layers.
- **Automated Deployment:** CI/CD automates application build and deployment workflows.
- **Centralized Monitoring:** CloudWatch provides centralized logging and monitoring.

## 11. Conclusion

This architecture demonstrates how to deploy and manage a production-oriented microservices application using Amazon ECS Fargate.

By combining containerization, private networking, load balancing, IAM-based security, automated CI/CD, and CloudWatch monitoring, the infrastructure provides a scalable and manageable environment for running containerized applications on AWS.

**Technologies:** AWS ECS, AWS Fargate, Amazon ECR, VPC, ALB, Route 53, WAF, ACM, NAT Gateway, IAM, Secrets Manager, CodePipeline, CodeBuild, CodeDeploy, S3, CloudWatch, Docker and GitHub.
