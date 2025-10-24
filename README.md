# Terraform AI Infrastructure Modules

Production-ready Terraform modules for deploying AI/ML workloads on AWS, GCP, and Azure.

## 🎯 What This Repo Contains

Reusable Terraform modules to deploy:
- **AI Applications** (FastAPI, LangChain, custom agents)
- **Platform Infrastructure** (CI/CD-ready, monitored, scalable)
- **Supporting Services** (databases, vector stores, caching)

All modules are production-tested and include:
✅ Auto-scaling
✅ Monitoring & logging
✅ Security best practices
✅ Cost optimization
✅ Multi-environment support

---

## 🚀 Quick Start

### Prerequisites

```bash
# Required tools
terraform >= 1.5.0
aws-cli / gcloud / azure-cli (for your chosen cloud)
kubectl (for Kubernetes modules)
```

### Example: Deploy AI Agent on AWS ECS

```bash
# 1. Clone repository
git clone https://github.com/Exidian-Tech/terraform-ai-infra.git
cd terraform-ai-infra/examples/aws-fastapi-agent

# 2. Configure variables
cp terraform.tfvars.example terraform.tfvars
# Edit terraform.tfvars with your settings

# 3. Initialize Terraform
terraform init

# 4. Plan deployment
terraform plan

# 5. Deploy
terraform apply
```

---

## 📦 Available Modules

### AWS Modules

#### 1. **ECS for AI Applications** (`modules/aws/ecs-ai-app`)

Deploy containerized AI apps on ECS Fargate with auto-scaling.

**What it creates:**
- ECS Cluster (Fargate)
- Application Load Balancer
- Auto-scaling policies
- CloudWatch logs
- Security groups
- IAM roles

**Use for:** FastAPI apps, LangChain agents, custom AI services

**Example usage:**
```hcl
module "ai_app" {
  source = "../../modules/aws/ecs-ai-app"
  
  app_name          = "my-ai-agent"
  container_image   = "your-registry/ai-agent:latest"
  container_port    = 8000
  cpu               = 512
  memory            = 1024
  desired_count     = 2
  
  environment = "production"
  vpc_id      = "vpc-xxxxx"
  subnet_ids  = ["subnet-xxxxx", "subnet-yyyyy"]
  
  # Auto-scaling
  min_capacity = 2
  max_capacity = 10
  
  # Environment variables for your app
  app_env_vars = {
    ANTHROPIC_API_KEY = "secret:api-key"
    DATABASE_URL      = "postgresql://..."
  }
}
```

---

#### 2. **Lambda + Bedrock** (`modules/aws/lambda-bedrock`)

Serverless AI agents using AWS Lambda and Bedrock.

**What it creates:**
- Lambda function
- API Gateway
- IAM roles for Bedrock access
- CloudWatch logs
- Optional S3 bucket for artifacts

**Use for:** Event-driven AI tasks, API endpoints, batch processing

**Example usage:**
```hcl
module "bedrock_agent" {
  source = "../../modules/aws/lambda-bedrock"
  
  function_name = "ai-summarizer"
  runtime       = "python3.11"
  handler       = "app.handler"
  
  # Your Lambda code
  source_dir = "${path.module}/lambda"
  
  # Bedrock model configuration
  bedrock_model_id = "anthropic.claude-3-sonnet-20240229-v1:0"
  
  # Environment variables
  environment_variables = {
    LOG_LEVEL = "INFO"
  }
  
  # API Gateway (optional)
  create_api_gateway = true
}
```

---

#### 3. **SageMaker Setup** (`modules/aws/sagemaker`)

Managed SageMaker infrastructure for model training and hosting.

**What it creates:**
- SageMaker notebook instances
- Training job configurations
- Endpoint configurations
- IAM roles
- S3 buckets for models/data

**Use for:** Model training, hosting custom models, ML experiments

---

### GCP Modules

#### 1. **Cloud Run for AI Apps** (`modules/gcp/cloud-run-ai`)

Deploy containerized AI applications on Cloud Run.

**What it creates:**
- Cloud Run service
- Load balancer (if needed)
- Cloud Logging
- Service account with permissions
- Secret Manager integration

**Use for:** HTTP-based AI agents, API services

**Example usage:**
```hcl
module "ai_service" {
  source = "../../modules/gcp/cloud-run-ai"
  
  project_id   = "my-gcp-project"
  region       = "us-central1"
  service_name = "ai-chatbot"
  
  container_image = "gcr.io/my-project/ai-chatbot:latest"
  
  # Resources
  cpu_limit    = "2"
  memory_limit = "2Gi"
  
  # Scaling
  min_instances = 1
  max_instances = 10
  
  # Environment variables
  env_vars = {
    ANTHROPIC_API_KEY = "secret:api-key"
    QDRANT_URL        = "https://qdrant.example.com"
  }
}
```

---

#### 2. **Vertex AI Platform** (`modules/gcp/vertex-ai`)

Setup Vertex AI for model training and deployment.

**What it creates:**
- Vertex AI workbench
- Training pipelines
- Model registry
- Endpoint configurations
- Service accounts

**Use for:** Training ML models, managed ML infrastructure

---

#### 3. **GKE with GPU** (`modules/gcp/gke-gpu`)

Kubernetes cluster with GPU support for ML workloads.

**What it creates:**
- GKE cluster with GPU node pool
- Horizontal pod autoscaler
- Cloud Logging integration
- Network policies
- Service mesh (optional)

**Use for:** Large-scale ML training, GPU-intensive AI workloads

---

### Azure Modules

#### 1. **Container Apps** (`modules/azure/container-apps`)

Deploy containerized AI apps on Azure Container Apps.

**What it creates:**
- Container App environment
- Container App instance
- Log Analytics workspace
- Managed identity
- Application Insights

**Use for:** Serverless AI applications, microservices

**Example usage:**
```hcl
module "ai_app" {
  source = "../../modules/azure/container-apps"
  
  resource_group_name = "ai-apps-rg"
  location            = "eastus"
  app_name            = "ai-assistant"
  
  container_image = "myregistry.azurecr.io/ai-assistant:latest"
  
  # Resources
  cpu    = 1.0
  memory = "2Gi"
  
  # Scaling
  min_replicas = 1
  max_replicas = 10
  
  # Environment variables
  env_vars = {
    OPENAI_API_KEY = "secret:api-key"
  }
}
```

---

#### 2. **Azure OpenAI Service** (`modules/azure/openai-service`)

Setup Azure OpenAI with deployments.

**What it creates:**
- Azure OpenAI account
- Model deployments (GPT-4, GPT-3.5)
- Private endpoints (optional)
- RBAC assignments

**Use for:** Azure-native OpenAI applications

---

#### 3. **AKS Setup** (`modules/azure/aks-setup`)

Azure Kubernetes Service for AI workloads.

**What it creates:**
- AKS cluster
- Node pools (CPU/GPU)
- Azure Monitor integration
- Network policies
- Ingress controller

**Use for:** Complex AI systems, multi-service architectures

---

## 📚 Complete Examples

### Example 1: AWS FastAPI AI Agent

Located in: `examples/aws-fastapi-agent/`

**What it deploys:**
- FastAPI application on ECS
- PostgreSQL RDS database
- Redis ElastiCache
- Application Load Balancer
- CloudWatch monitoring

**Files:**
