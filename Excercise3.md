# Exercise 3: Deploy Application to AKS using GitHub Copilot

In this exercise, you will deploy the 4-microservice application to your AKS cluster using GitHub Copilot's assistance and learn how to verify the deployment status.

<details>
<summary><b>Step 1: Navigate to the Manifest Directory</b></summary>

1. Ask GitHub Copilot:

```
@terminal Navigate to the directory containing aks-store-quickstart.yaml file
```

2. Copilot might suggest:

```bash
cd aks-store-demo
# or if you're in a different location
cd src
```

3. Verify the file exists:

```bash
ls aks-store-quickstart.yaml
```

</details>

<details>
<summary><b>Step 2: Deploy the Application using GitHub Copilot</b></summary>

1. In Copilot Chat, ask:

```
@terminal Deploy the Kubernetes application using the aks-store-quickstart.yaml manifest file
```

2. GitHub Copilot should suggest:

```bash
kubectl apply -f aks-store-quickstart.yaml
```

3. Execute the command. You should see output similar to:

```
statefulset.apps/rabbitmq created
configmap/rabbitmq-enabled-plugins created
service/rabbitmq created
deployment.apps/order-service created
service/order-service created
deployment.apps/product-service created
service/product-service created
deployment.apps/store-front created
service/store-front created
```

</details>

<details>
<summary><b>Step 3: Monitor Deployment Progress using GitHub Copilot</b></summary>

1. Ask GitHub Copilot:

```
@terminal How do I check if all my Kubernetes pods are running?
```

2. Copilot should suggest:

```bash
kubectl get pods
```

3. You can also ask:

```
@terminal Watch the pods status in real-time
```

Expected response:

```bash
kubectl get pods --watch
```

4. Wait until all 4 pods show "Running" status and "1/1" in the READY column:

```
NAME                               READY   STATUS    RESTARTS   AGE
rabbitmq-0                         1/1     Running   0          2m
order-service-6c8b4d6f9b-xyz       1/1     Running   0          2m
product-service-5d9c7f8d4-abc      1/1     Running   0          2m
store-front-7b8c9d6f5e-def         1/1     Running   0          2m
```

> [!NOTE]
> You should see exactly 4 pods: 1 RabbitMQ, 1 order-service, 1 product-service, and 1 store-front

5. Press `Ctrl+C` to stop watching.

> [!TIP]
> If a pod is not starting, ask Copilot: `@terminal Show me the logs for the pod that's failing`

</details>

<details>
<summary><b>Step 4: Verify Services using GitHub Copilot</b></summary>

1. Ask Copilot:

```
@terminal Show all Kubernetes services and their types
```

2. Copilot should suggest:

```bash
kubectl get services
```

Or for more detail:

```bash
kubectl get svc -o wide
```

3. You should see output like:

```
NAME              TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)
kubernetes        ClusterIP      10.0.0.1       <none>         443/TCP
rabbitmq          ClusterIP      10.0.100.1     <none>         5672/TCP,15672/TCP
order-service     ClusterIP      10.0.100.2     <none>         3000/TCP
product-service   ClusterIP      10.0.100.3     <none>         3002/TCP
store-front       LoadBalancer   10.0.100.4     <pending>      80:30080/TCP
```

4. The `store-front` service is type `LoadBalancer`. Ask Copilot:

```
@terminal Why is the EXTERNAL-IP showing as <pending> for my LoadBalancer service?
```

Copilot will explain that Azure is provisioning a public IP, which takes 2-3 minutes.

</details>

### Summary
✅ Application deployed to AKS  
✅ All pods are running successfully  
✅ Services are created  
✅ LoadBalancer is being provisioned  

---
