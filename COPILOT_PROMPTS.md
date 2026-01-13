# GitHub Copilot Prompts for Dockerization and Azure Container Apps Deployment

This guide provides comprehensive prompts for using GitHub Copilot to generate Dockerfiles, build images with Azure Container Registry (ACR), and deploy microservices to Azure Container Apps.

---

## Table of Contents
- [Phase 1: Generate Dockerfiles](#phase-1-generate-dockerfiles)
- [Phase 2: Build Images with ACR](#phase-2-build-images-with-acr)
- [Phase 3: Deploy to Azure Container Apps](#phase-3-deploy-to-azure-container-apps)
- [Phase 4: Setup Infrastructure](#phase-4-setup-infrastructure)
- [Phase 5: Advanced Configuration](#phase-5-advanced-configuration)

---

## Phase 1: Generate Dockerfiles

### 1.1 AI Service (Python/FastAPI)
```
Create a production-ready multi-stage Dockerfile for a Python FastAPI microservice:
- Base image: python:3.11-slim for build, python:3.11-slim for runtime
- Working directory: /app
- Dependencies from requirements.txt include: fastapi==0.115.1, uvicorn==0.34.0, openai==1.66.3, azure.identity==1.21.0, pydantic==2.10.6, pyyaml==6.0.2
- Copy main.py and routers/ directory containing __init__.py, description_generator.py, image_generator.py
- Install dependencies with pip install --no-cache-dir -r requirements.txt
- Expose port 5001
- Run with: uvicorn main:app --host 0.0.0.0 --port 5001
- Create non-root user 'appuser' for security
- Add health check: curl http://localhost:5001/health
- Optimize image size by removing unnecessary files
```

### 1.2 Order Service (Node.js/Fastify)
```
Create a production-ready multi-stage Dockerfile for a Node.js Fastify microservice:
- Build stage: node:20-alpine
- Production stage: node:20-alpine
- Working directory: /usr/src/app
- Dependencies from package.json include: fastify@^4.0.0, @azure/service-bus@^7.9.4, @fastify/cors@^8.3.0, rhea@^3.0.2
- Copy package*.json and run npm ci --only=production
- Copy app.js, routes/, plugins/ directories
- Expose port 3000
- Create non-root nodejs user (uid 1001)
- Add health check: HTTP GET on localhost:3000
- Run with: node app.js
- Clean npm cache to reduce image size
```

### 1.3 Makeline Service (Go/Gin)
```
Create a production-ready multi-stage Dockerfile for a Go Gin microservice:
- Build stage: golang:1.24-alpine with build tools
- Runtime stage: alpine:latest
- Working directory: /app
- Go module: aks-store-demo/makeline-service with go 1.24.0
- Dependencies include: gin-gonic/gin@v1.10.0, Azure Cosmos DB SDK, Azure Service Bus SDK, MongoDB driver
- Copy go.mod and go.sum, run go mod download
- Copy all .go files: main.go, orders.go, orderqueue.go, mongodb.go, cosmosdb.go
- Build with CGO_ENABLED=0 GOOS=linux for static binary
- Install ca-certificates in runtime stage for Azure SDK
- Expose port 3001
- Create non-root user 'appuser' (uid 1001)
- Add health check endpoint support
- Run binary: ./makeline-service
```

### 1.4 Product Service (Rust/Actix)
```
Create a production-ready multi-stage Dockerfile for a Rust Actix-web microservice with WebAssembly support:
- Build stage: rust:1.75-bookworm
- Runtime stage: debian:bookworm-slim
- Working directory: /app
- Copy Cargo.toml, Cargo.lock
- Dependencies: actix-web@4.8.0, actix-cors@0.7.0, wasmtime (git), serde@1.0.204, reqwest@0.12.5
- Copy src/ directory, crates/ (sample-rule, second-rule), tests/
- Copy config.yml and rule_engine.wit for WASM support
- Build release with: cargo build --release
- Install libssl and ca-certificates in runtime stage
- Copy binary from /app/target/release/product-service
- Expose port 3002
- Create non-root user 'appuser'
- Add health check
- Run: ./product-service
```

### 1.5 Store-Front (Vue.js/Vite)
```
Create a production-ready multi-stage Dockerfile for a Vue.js 3 SPA with Vite and nginx:
- Build stage: node:20-alpine
- Working directory: /app
- Dependencies: vue@^3.5.13, pinia@^3.0.1, vue-router@^4.5.0, vite, typescript
- Copy package*.json and run npm ci
- Copy src/, public/, index.html, vite.config.ts, tsconfig*.json files
- Build production bundle: npm run build
- Runtime stage: nginx:1.25-alpine
- Copy nginx.conf for SPA routing (handle Vue Router history mode with try_files)
- Copy dist/ to /usr/share/nginx/html
- Configure nginx to rewrite /api/* requests to backend services
- Expose port 8080
- Add health check on nginx status
- Run nginx in foreground
```

### 1.6 Store-Admin (Vue.js/Vite)
```
Create a production-ready multi-stage Dockerfile for a Vue.js 3 admin dashboard:
- Build stage: node:20-alpine
- Working directory: /app
- Same dependencies as store-front: vue@^3.5.13, pinia@^3.0.1, vue-router@^4.5.0, typescript@~5.8.0
- Copy package*.json, run npm ci
- Copy src/, public/, index.html, vite.config.ts, all tsconfig files
- Build: npm run build
- Runtime stage: nginx:1.25-alpine
- Copy custom nginx.conf with SPA routing support
- Copy built assets from dist/ to /usr/share/nginx/html
- Expose port 8081
- Configure API proxy rules in nginx
- Add health check
- Run nginx daemon off
```

### 1.7 Virtual Customer (Rust CLI)
```
Create a production-ready multi-stage Dockerfile for a Rust CLI simulation tool:
- Build stage: rust:1.75-bookworm
- Working directory: /app
- Copy Cargo.toml, Cargo.lock, src/main.rs
- Build release binary: cargo build --release
- Runtime stage: debian:bookworm-slim
- Install ca-certificates and libssl
- Copy binary from /app/target/release/virtual-customer
- Create non-root user
- Set entrypoint: ./virtual-customer
- No exposed ports (CLI tool)
```

### 1.8 Virtual Worker (Rust CLI)
```
Create a production-ready multi-stage Dockerfile for a Rust worker service:
- Build stage: rust:1.75-bookworm
- Working directory: /app
- Copy Cargo.toml, Cargo.lock, src/main.rs
- Build optimized release: cargo build --release
- Runtime stage: debian:bookworm-slim
- Install runtime dependencies: ca-certificates, libssl
- Copy binary: ./virtual-worker
- Create non-root user for security
- Set entrypoint and CMD
- Add environment variable support
```

### 1.9 .dockerignore Files
```
Create optimized .dockerignore files for each service to exclude unnecessary files from Docker build context:

For Node.js services (order-service, store-front, store-admin):
- node_modules, .npm, npm-debug.log
- dist/, build/, .cache/
- .env, .env.local, .env.*.local
- *.log, logs/
- .git/, .gitignore, README.md
- test/, tests/, coverage/
- .vscode/, .idea/
- *.md (except keep specific ones if needed)

For Python service (ai-service):
- __pycache__/, *.pyc, *.pyo, *.pyd
- .Python, env/, venv/, .venv/
- *.log, .pytest_cache/
- .git/, .gitignore, README.md
- .env, .env.local

For Go service (makeline-service):
- *.exe, *.dll, *.so, *.dylib
- vendor/
- .git/, .gitignore
- *.log
- .env

For Rust services (product-service, virtual-customer, virtual-worker):
- target/
- **/*.rs.bk
- Cargo.lock (include only if binary)
- .git/, .gitignore
- *.log
```

---

## Phase 2: Build Images with ACR

### 2.1 ACR Setup and Authentication
```
Generate Azure CLI commands to:
1. Create an Azure Container Registry named 'aksstoreacr' in resource group 'aks-store-rg'
2. Use Basic SKU for cost optimization (or Standard/Premium for production)
3. Enable admin user for simple authentication
4. Login to the registry using az acr login
5. Get the login server URL for tagging images
6. Store credentials securely for CI/CD pipelines
```

### 2.2 Build All Service Images with ACR
```
Create a PowerShell script (build-all-images.ps1) that:
1. Defines variables: ACR_NAME, RESOURCE_GROUP, VERSION_TAG
2. Uses az acr build to build and push all 8 microservice images:
   - ai-service (from src/ai-service with tag: ai-service:$VERSION and ai-service:latest)
   - order-service (from src/order-service with tag: order-service:$VERSION)
   - makeline-service (from src/makeline-service)
   - product-service (from src/product-service)
   - store-front (from src/store-front)
   - store-admin (from src/store-admin)
   - virtual-customer (from src/virtual-customer)
   - virtual-worker (from src/virtual-worker)
3. Include error handling and logging for each build
4. Display build summary with image names and tags
5. Optionally run az acr repository list to verify all images
6. Include --platform linux/amd64 flag for multi-architecture support
```

### 2.3 Individual ACR Build Commands
```
Generate individual az acr build commands for each microservice with the following format:

az acr build \
  --registry <acr-name> \
  --resource-group <resource-group> \
  --image <service-name>:{{.Run.ID}} \
  --image <service-name>:latest \
  --file <path-to-dockerfile> \
  --platform linux/amd64 \
  <context-path>

Include build arguments for:
- APP_VERSION
- BUILD_DATE
- GIT_COMMIT_SHA (if available)

Add tags for versioning:
- :latest (always latest)
- :v1.0.0 (semantic version)
- :{{.Run.ID}} (unique ACR build ID)
- :$(git rev-parse --short HEAD) (git commit hash)
```

### 2.4 ACR Task for Automated Builds
```
Create an ACR task YAML file (acr-task.yaml) for automated CI/CD that:
1. Triggers on git push to main branch
2. Builds all microservice images in parallel where possible
3. Tags images with git commit SHA and semantic version
4. Runs security scans on built images
5. Pushes to ACR only if build succeeds
6. Sends notifications on build completion/failure
7. Includes steps for multi-stage builds
8. Supports build caching for faster builds

Generate the az acr task create command to register this task.
```

---

## Phase 3: Deploy to Azure Container Apps

### 3.1 Container Apps Environment Setup
```
Generate Bicep/ARM template or Azure CLI commands to:
1. Create Azure Container Apps Environment named 'aks-store-env' in resource group 'aks-store-rg'
2. Configure in region: eastus (or user-specified)
3. Create Log Analytics workspace for monitoring: 'aks-store-logs'
4. Enable Dapr for microservices communication
5. Configure virtual network integration (optional for production)
6. Set up zone redundancy for high availability
7. Configure internal/external environment based on security requirements
```

### 3.2 Deploy AI Service Container App
```
Create Azure CLI or Bicep code to deploy ai-service container app:
- Name: ai-service
- Image: <acr-name>.azurecr.io/ai-service:latest
- Enable ingress: external, port 5001
- Set CPU: 0.5, Memory: 1Gi
- Min replicas: 1, Max replicas: 5
- Configure autoscaling rules: HTTP request count > 100
- Environment variables:
  * OPENAI_API_KEY (from Azure Key Vault secret)
  * OPENAI_ENDPOINT (from config)
  * AZURE_CLIENT_ID (managed identity)
- Enable managed identity for Azure service authentication
- Configure health probes: liveness on /health, readiness on /ready
- Set revision mode to single for rolling updates
```

### 3.3 Deploy Order Service Container App
```
Create deployment configuration for order-service:
- Name: order-service
- Image from ACR: order-service:latest
- Enable ingress: external, target port 3000
- Resources: CPU 0.5, Memory 1Gi
- Scaling: min 2, max 10 replicas
- Scale rules: HTTP concurrent requests > 50
- Environment variables:
  * ORDER_QUEUE_HOSTNAME (Azure Service Bus namespace)
  * ORDER_QUEUE_CONNECTION_STRING (from Key Vault)
  * MAKELINE_SERVICE_URL (internal URL to makeline-service)
- Enable Dapr sidecar:
  * App ID: order-service
  * App port: 3000
  * Enable pub/sub component
- Configure CORS for store-front and store-admin
- Set session affinity if needed
```

### 3.4 Deploy Makeline Service Container App
```
Generate deployment spec for makeline-service:
- Name: makeline-service
- Image: <acr>.azurecr.io/makeline-service:latest
- Ingress: internal only, port 3001
- Resources: CPU 0.75, Memory 1.5Gi
- Scaling: min 1, max 8
- Environment variables:
  * ORDER_QUEUE_URI (Service Bus connection)
  * ORDER_DB_URI (Cosmos DB or MongoDB connection string)
  * ORDER_DB_NAME (database name)
  * ORDER_DB_COLLECTION_NAME
- Enable managed identity for Cosmos DB access
- Configure Dapr for state management
- Set up Service Bus binding for order queue
- Add persistent volume for temporary data (if needed)
- Configure connection to Azure Monitor
```

### 3.5 Deploy Product Service Container App
```
Create deployment for product-service with WASM support:
- Name: product-service
- Image: product-service:latest from ACR
- Ingress: internal, port 3002
- Resources: CPU 1.0, Memory 2Gi (higher for WASM runtime)
- Scaling: min 2, max 6
- Environment variables:
  * AI_SERVICE_URL (internal URL)
  * WASM_MODULE_PATH (if using external WASM modules)
- Mount Azure Files volume for WASM modules (if dynamic loading)
- Enable Dapr for service discovery
- Configure HTTP/gRPC probes
- Set resource limits for WASM execution
```

### 3.6 Deploy Store-Front Container App
```
Deploy store-front Vue.js SPA:
- Name: store-front
- Image: store-front:latest
- Ingress: external, port 8080, allow insecure traffic for dev
- Resources: CPU 0.25, Memory 0.5Gi
- Scaling: min 2, max 10
- Environment variables at build time (already in image):
  * VUE_APP_ORDER_SERVICE_URL
  * VUE_APP_PRODUCT_SERVICE_URL
- Configure custom domain (optional)
- Enable CDN integration for static assets
- Set up traffic splitting for A/B testing
- Configure session affinity: none (stateless)
```

### 3.7 Deploy Store-Admin Container App
```
Deploy store-admin dashboard:
- Name: store-admin
- Image: store-admin:latest
- Ingress: external with authentication, port 8081
- Resources: CPU 0.25, Memory 0.5Gi
- Scaling: min 1, max 3
- Configure Azure AD authentication for admin access
- Environment variables (build-time):
  * VUE_APP_ORDER_SERVICE_URL
  * VUE_APP_MAKELINE_SERVICE_URL
- Enable IP restrictions for admin access
- Configure CORS for secure API calls
```

### 3.8 Deploy Virtual Customer and Worker
```
Deploy virtual-customer and virtual-worker as jobs (not services):
- Use Azure Container Apps Jobs instead of regular container apps
- Schedule virtual-customer to run every 5 minutes (cron schedule)
- Virtual-worker runs on event trigger (Service Bus queue depth)
- Configure job execution:
  * Timeout: 300 seconds
  * Retry policy: 3 attempts
  * Parallelism: 1
- Environment variables for endpoint URLs
- Use managed identity for authentication
```

### 3.9 Complete Deployment Script
```
Create a comprehensive PowerShell/Bash deployment script (deploy-to-aca.ps1 or .sh) that:
1. Checks prerequisites: Azure CLI installed, logged in, subscription set
2. Creates resource group if not exists
3. Creates Container Apps environment
4. Creates Log Analytics workspace and links to environment
5. Creates Azure Service Bus namespace and queue
6. Creates Cosmos DB account and database
7. Creates Azure Key Vault and stores secrets
8. Deploys all 8 container apps in correct order (dependencies first)
9. Configures Dapr components for pub/sub and state management
10. Sets up monitoring and alerts
11. Outputs all service URLs and endpoints
12. Runs smoke tests to verify deployments
13. Includes rollback capability on failure
```

---

## Phase 4: Setup Infrastructure

### 4.1 Azure Service Bus Configuration
```
Generate Bicep or Azure CLI commands to create Azure Service Bus infrastructure:
1. Create Service Bus namespace: 'aks-store-sb' (Standard or Premium tier)
2. Create queue: 'orders' with:
   - Max delivery count: 10
   - Lock duration: 30 seconds
   - Enable dead letter queue
   - Enable sessions: false
   - Max size: 1GB
3. Create topic: 'order-events' with subscriptions:
   - 'makeline-subscription' for makeline-service
   - 'notification-subscription' for future use
4. Configure authorization rules with least privilege
5. Enable managed identity access for container apps
6. Set up diagnostic settings to send logs to Log Analytics
```

### 4.2 Database Setup (Cosmos DB)
```
Create Cosmos DB infrastructure for makeline-service:
1. Create Cosmos DB account: 'aks-store-cosmos'
   - API: MongoDB (compatible with existing code)
   - Consistency level: Session
   - Enable multi-region writes: false (single region for cost)
   - Enable automatic failover: true
2. Create database: 'orderdb'
3. Create collection: 'orders' with:
   - Partition key: /orderId
   - Throughput: 400 RU/s autoscale
   - Indexing policy for common queries
4. Configure firewall to allow Container Apps subnet
5. Enable diagnostic logging
6. Grant managed identity access with RBAC
```

### 4.3 Key Vault for Secrets Management
```
Setup Azure Key Vault for secure secrets management:
1. Create Key Vault: 'aks-store-kv'
2. Enable RBAC authorization model
3. Store secrets:
   - cosmos-connection-string
   - servicebus-connection-string
   - openai-api-key
   - acr-credentials
4. Grant Container Apps managed identities "Key Vault Secrets User" role
5. Configure Key Vault references in Container Apps:
   - Use secretRef with Key Vault URI
   - Enable automatic secret rotation
6. Set up access policies for developers (limited read)
```

### 4.4 Monitoring and Observability
```
Configure comprehensive monitoring solution:
1. Log Analytics Workspace: collect all container logs
2. Application Insights: 
   - Create instance: 'aks-store-insights'
   - Enable for all services
   - Configure connection strings in environment variables
   - Set up custom metrics and traces
3. Container Apps monitoring:
   - Enable system logs and application logs
   - Configure log retention: 90 days
   - Set up log queries for common scenarios
4. Create Azure Monitor Workbook with dashboards for:
   - Service health and availability
   - Request rates and latencies
   - Error rates and exceptions
   - Resource utilization (CPU, memory)
5. Configure alerts:
   - High error rate (>5%)
   - Container restart events
   - Scaling events
   - Service availability < 99%
```

### 4.5 Complete Infrastructure as Code
```
Create a comprehensive Bicep template (main.bicep) that deploys:
1. Module structure:
   - modules/acr.bicep (Container Registry)
   - modules/containerapp-env.bicep (Container Apps Environment)
   - modules/servicebus.bicep (Service Bus namespace and queues)
   - modules/cosmosdb.bicep (Cosmos DB)
   - modules/keyvault.bicep (Key Vault)
   - modules/monitoring.bicep (Log Analytics, App Insights)
   - modules/containerapp.bicep (Reusable container app definition)
2. Parameter file (main.parameters.json) for environment-specific configs
3. Use managed identities throughout (no connection strings in env vars)
4. Output all important values: URLs, connection strings, resource IDs
5. Include tags for cost tracking and governance
6. Use latest API versions for all resources
7. Enable diagnostic settings on all resources
8. Configure private endpoints for production environments
```

---

## Phase 5: Advanced Configuration

### 5.1 Dapr Configuration for Microservices
```
Create Dapr component configurations for Container Apps:
1. State store component (Cosmos DB):
   - Component name: statestore
   - Type: state.azure.cosmosdb
   - Connect using managed identity
   - Configure for makeline-service
2. Pub/Sub component (Service Bus):
   - Component name: orderpubsub
   - Type: pubsub.azure.servicebus
   - Topics: order-created, order-completed
   - Subscriptions for each service
3. Service-to-service invocation:
   - Enable mTLS between services
   - Configure retry policies
   - Set timeout values
4. Generate YAML files for each component
5. Apply using: az containerapp env dapr-component set
```

### 5.2 Networking and Security
```
Configure advanced networking for production:
1. Create Virtual Network with subnets:
   - Container Apps subnet: /23 (512 IPs)
   - Private endpoints subnet: /27
   - Application Gateway subnet: /27
2. Enable VNet integration for Container Apps environment
3. Configure NSG rules:
   - Allow inbound only from Application Gateway
   - Allow outbound to Azure services via service tags
4. Set up private endpoints for:
   - Azure Container Registry
   - Cosmos DB
   - Service Bus
   - Key Vault
5. Configure Azure Front Door or Application Gateway:
   - SSL termination
   - WAF rules
   - Custom domain routing
   - Traffic distribution
```

### 5.3 CI/CD Pipeline Configuration
```
Create GitHub Actions workflow (.github/workflows/deploy.yml) that:
1. Triggers on push to main branch
2. Authenticates to Azure using OIDC (no secrets)
3. Builds all Docker images using ACR tasks
4. Runs security scanning (Trivy, Snyk) on images
5. Deploys infrastructure using Bicep (if changed)
6. Updates container apps with new image tags
7. Implements blue-green deployment strategy
8. Runs integration tests after deployment
9. Automatically rollback on test failures
10. Sends notifications to Teams/Slack
11. Includes manual approval gate for production
12. Tags releases and updates documentation

Alternative: Create Azure DevOps pipeline (azure-pipelines.yml) with same capabilities
```

### 5.4 Autoscaling Configuration
```
Configure advanced autoscaling rules for each service:
1. HTTP scaling rule (for external services):
   - Metric: concurrent requests
   - Threshold: 100 requests
   - Scale up: add 2 replicas
   - Scale down: remove 1 replica after 5 minutes
2. CPU-based scaling:
   - Trigger: CPU > 70%
   - Min replicas: 2, Max: 10
3. Memory-based scaling:
   - Trigger: Memory > 80%
4. Custom metrics (Application Insights):
   - Queue depth for async services
   - Response time > 500ms
5. Schedule-based scaling:
   - Scale up during business hours
   - Scale down at night
6. Generate Azure CLI commands to apply these rules
```

### 5.5 Backup and Disaster Recovery
```
Create disaster recovery plan and configuration:
1. Cosmos DB:
   - Enable automatic backup (continuous backup mode)
   - Configure point-in-time restore
   - Set up geo-replication to secondary region
2. Container Apps:
   - Deploy to multiple regions with Traffic Manager
   - Use Azure Front Door for global load balancing
   - Implement health checks for automatic failover
3. Configuration backup:
   - Export all Container App configurations
   - Store in Git repository
   - Automate configuration backup script
4. Create recovery runbook with:
   - RTO (Recovery Time Objective): 1 hour
   - RPO (Recovery Point Objective): 5 minutes
   - Step-by-step recovery procedures
   - Test recovery plan quarterly
```

### 5.6 Cost Optimization
```
Implement cost optimization strategies:
1. Right-size container resources:
   - Analyze actual CPU/Memory usage
   - Adjust resource allocations
   - Use spot instances for non-critical workloads
2. Configure consumption-based scaling:
   - Scale to zero for dev/test environments
   - Min replicas = 0 for non-critical services
3. Use Azure Advisor recommendations
4. Set up cost alerts:
   - Daily spend > $50
   - Monthly spend > $1000
5. Tag resources for cost allocation
6. Review ACR storage and clean old images
7. Use reserved instances for predictable workloads
```

### 5.7 Testing and Validation
```
Create comprehensive testing scripts:
1. Health check script (test-health.ps1):
   - Curl all service health endpoints
   - Verify response codes and times
   - Check Dapr sidecar health
2. Integration test suite:
   - Create order through order-service
   - Verify order appears in makeline-service
   - Test product service responses
   - Verify AI service integration
3. Load testing with Azure Load Testing:
   - Simulate 1000 concurrent users
   - Test autoscaling behavior
   - Identify bottlenecks
4. Chaos engineering tests:
   - Kill random pods
   - Inject network latency
   - Simulate region failure
5. Security testing:
   - Scan for vulnerabilities
   - Test authentication/authorization
   - Verify secrets are not exposed
```

---

## Quick Reference Commands

### Build All Images
```bash
# Set variables
$ACR_NAME = "aksstoreacr"
$VERSION = "v1.0.0"

# Build all services
az acr build --registry $ACR_NAME --image ai-service:$VERSION --image ai-service:latest --file src/ai-service/Dockerfile src/ai-service
az acr build --registry $ACR_NAME --image order-service:$VERSION --image order-service:latest --file src/order-service/Dockerfile src/order-service
az acr build --registry $ACR_NAME --image makeline-service:$VERSION --image makeline-service:latest --file src/makeline-service/Dockerfile src/makeline-service
az acr build --registry $ACR_NAME --image product-service:$VERSION --image product-service:latest --file src/product-service/Dockerfile src/product-service
az acr build --registry $ACR_NAME --image store-front:$VERSION --image store-front:latest --file src/store-front/Dockerfile src/store-front
az acr build --registry $ACR_NAME --image store-admin:$VERSION --image store-admin:latest --file src/store-admin/Dockerfile src/store-admin
```

### Deploy All Container Apps
```bash
# Deploy services
az containerapp create --name ai-service --resource-group aks-store-rg --environment aks-store-env --image $ACR_NAME.azurecr.io/ai-service:latest --target-port 5001 --ingress external
az containerapp create --name order-service --resource-group aks-store-rg --environment aks-store-env --image $ACR_NAME.azurecr.io/order-service:latest --target-port 3000 --ingress external
az containerapp create --name makeline-service --resource-group aks-store-rg --environment aks-store-env --image $ACR_NAME.azurecr.io/makeline-service:latest --target-port 3001 --ingress internal
az containerapp create --name product-service --resource-group aks-store-rg --environment aks-store-env --image $ACR_NAME.azurecr.io/product-service:latest --target-port 3002 --ingress internal
az containerapp create --name store-front --resource-group aks-store-rg --environment aks-store-env --image $ACR_NAME.azurecr.io/store-front:latest --target-port 8080 --ingress external
az containerapp create --name store-admin --resource-group aks-store-rg --environment aks-store-env --image $ACR_NAME.azurecr.io/store-admin:latest --target-port 8081 --ingress external
```

---

## Additional Resources

### Documentation Links to Include
- Azure Container Apps documentation
- Azure Container Registry best practices
- Dapr documentation for Container Apps
- Multi-stage Docker builds
- Container security best practices
- Azure monitoring and logging
- Bicep language reference

### Next Steps After Deployment
1. Configure custom domains and SSL certificates
2. Set up Azure API Management for API gateway
3. Implement rate limiting and throttling
4. Configure Azure AD B2C for customer authentication
5. Set up Azure Cache for Redis for session management
6. Implement distributed tracing with Application Insights
7. Configure backup and disaster recovery
8. Set up cost management and budgets

---

**Note**: Replace placeholders like `<acr-name>`, `<resource-group>`, and URLs with your actual values. Always test in a development environment before deploying to production.
