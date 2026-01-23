# Exercise 2: Azure Resources Setup using GitHub Copilot

In this exercise, you will use GitHub Copilot to help you connect to the pre-provisioned Azure resources including the resource group and AKS cluster.

## Verify Azure Resources

1. Login to the [Azure Portal](https://portal.azure.com) using the credentials available in the **Resources** tab.
   - **Username**: `<from resources tab>`
   - **Password**: `<from resources tab>`

2. Once logged in, you will see the Resource group named **"TechConnect2026"** containing your pre-provisioned AKS cluster.

   ![Azure Resources](azureresources.png)

3. Note the **AKS cluster name** from the Azure Portal - you will need it in the next step.

<details>
<summary><b>Step 1: Get AKS Credentials using GitHub Copilot</b></summary>

1. Ask GitHub Copilot:

```
@terminal How do I get kubectl credentials for my AKS cluster in resource group 'TechConnect2026'?
```

2. Copilot should suggest:

```bash
az aks get-credentials --resource-group TechConnect2026 --name <your-aks-cluster-name>
```

3. Replace `<your-aks-cluster-name>` with the AKS cluster name you noted from the Azure Portal, then execute the command to configure kubectl to use your AKS cluster.

4. Verify connectivity:

```bash
kubectl get nodes
```

You should see 3 nodes in "Ready" state.

</details>

<details>
<summary><b>Step 2: Inspect the Application Manifest using GitHub Copilot</b></summary>

1. Open the file `aks-store-app-manifest/aks-store-quickstart.yaml` in VS Code.

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
✅ kubectl is configured to access the pre-provisioned AKS cluster  
✅ You understand the application architecture  

---