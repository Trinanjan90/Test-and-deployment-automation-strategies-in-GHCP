# Exercise 2: Azure Resources Setup using GitHub Copilot

In this exercise, you will use GitHub Copilot to help you create the necessary Azure resources including a resource group and an AKS cluster.

<details>
<summary><b>Step 1: Ask GitHub Copilot for Resource Group Creation</b></summary>

1. Open GitHub Copilot Chat and ask:

```
@terminal Create an Azure resource group named 'aks-store-rg' in the eastus region
```

2. Copilot should suggest a command similar to:

```bash
az group create --name aks-store-rg --location eastus
```

3. Execute the command in the terminal.
4. Verify the resource group was created:

```bash
az group show --name aks-store-rg
```

</details>

<details>
<summary><b>Step 2: Use GitHub Copilot to Create AKS Cluster</b></summary>

1. In Copilot Chat, provide a detailed request:

```
@terminal Create an AKS cluster named 'aks-store-cluster' in resource group 'aks-store-rg' with the following specifications:
- Node count: 3
- Node VM size: Standard_DS2_v2  
- Enable managed identity
- Enable monitoring
- Kubernetes version: 1.28 or latest
```

2. GitHub Copilot should suggest something like:

```bash
az aks create \
  --resource-group aks-store-rg \
  --name aks-store-cluster \
  --node-count 3 \
  --node-vm-size Standard_DS2_v2 \
  --enable-managed-identity \
  --enable-addons monitoring \
  --generate-ssh-keys \
  --location eastus
```

3. Execute the command. **This will take approximately 5-10 minutes to complete.**

> [!NOTE]
> While the AKS cluster is being created, you can read through the next steps to understand what we'll be deploying.

4. Once complete, verify the cluster:

```bash
az aks show --resource-group aks-store-rg --name aks-store-cluster --output table
```

</details>

<details>
<summary><b>Step 3: Get AKS Credentials using GitHub Copilot</b></summary>

1. Ask GitHub Copilot:

```
@terminal How do I get kubectl credentials for my AKS cluster named 'aks-store-cluster' in resource group 'aks-store-rg'?
```

2. Copilot should suggest:

```bash
az aks get-credentials --resource-group aks-store-rg --name aks-store-cluster
```

3. Execute the command to configure kubectl to use your AKS cluster.

4. Verify connectivity:

```bash
kubectl get nodes
```

You should see 3 nodes in "Ready" state.

</details>

<details>
<summary><b>Step 4: Inspect the Application Manifest using GitHub Copilot</b></summary>

1. Open the file `aks-store-quickstart.yaml` in VS Code.

2. Select some portion of the YAML and ask Copilot:

```
@workspace Explain what this Kubernetes manifest deploys
```

3. Copilot will explain that this manifest deploys:
   - **RabbitMQ StatefulSet**: Message broker for async order processing (with management console on port 15672)
   - **Order-Service Deployment**: Node.js API for handling customer orders (port 3000)
   - **Product-Service Deployment**: Rust API for managing product catalog (port 3002)
   - **Store-Front Deployment**: Vue.js customer-facing web application (port 8080)
   - **Services**: 3 ClusterIP services for internal communication and 1 LoadBalancer for external access
   - **ConfigMap**: RabbitMQ plugin configuration
   - **Health Probes**: Startup, readiness, and liveness probes for all services

4. You can ask follow-up questions like:

```
@workspace What environment variables does the order-service need to connect to RabbitMQ?
```

```
@workspace Why is RabbitMQ deployed as a StatefulSet instead of a Deployment?
```

```
@workspace What health check endpoints are configured for these services?
```

</details>

### Summary
✅ Resource group created  
✅ AKS cluster is running with 3 nodes  
✅ kubectl is configured to access the cluster  
✅ You understand the application architecture  

---