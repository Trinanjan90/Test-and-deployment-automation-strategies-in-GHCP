# Exercise 4: Automated Testing - Verify Deployment and Get Application URL

In this exercise, you will use GitHub Copilot to create automated test scripts that verify deployment status, retrieve the application URL, and validate that all services are functioning correctly.

<details>
<summary><b>Step 1: Create a Shell Script using GitHub Copilot</b></summary>

1. Ask GitHub Copilot:

```
Create a bash script that waits for the store-front LoadBalancer service to get an external IP and then displays the application URL
```

2. Copilot should generate a script. Create a file named `get-app-url.sh`:

```bash
#!/bin/bash

echo "Waiting for store-front service to get an external IP..."
echo "This may take 2-3 minutes..."

# Wait for the EXTERNAL-IP to be assigned
while true; do
  EXTERNAL_IP=$(kubectl get service store-front -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
  
  if [ -n "$EXTERNAL_IP" ] && [ "$EXTERNAL_IP" != "<pending>" ]; then
    echo ""
    echo "✅ Deployment successful!"
    echo "=========================================="
    echo "🌐 Application URL: http://$EXTERNAL_IP"
    echo "=========================================="
    echo ""
    echo "Open this URL in your browser to access the AKS Store Demo application."
    break
  fi
  
  echo -n "."
  sleep 5
done
```

3. Ask Copilot how to make it executable:

```
@terminal How do I make this bash script executable?
```

Copilot will suggest:

```bash
chmod +x get-app-url.sh
```

</details>

<details>
<summary><b>Step 2: Run the Script using GitHub Copilot</b></summary>

1. Execute the script:

```bash
./get-app-url.sh
```

2. The script will wait and display dots while the LoadBalancer IP is being assigned.

3. Once ready, you'll see output like:

```
✅ Deployment successful!
==========================================
🌐 Application URL: http://20.12.34.56
==========================================

Open this URL in your browser to access the AKS Store Demo application.
```

> [!NOTE]
> Keep this IP address handy. You'll need it to access the application.

</details>

<details>
<summary><b>Step 3: Alternative Method - Use kubectl Directly with GitHub Copilot</b></summary>

1. If you prefer not to use a script, ask Copilot:

```
@terminal Show me the external IP of my store-front service
```

2. Copilot might suggest:

```bash
kubectl get service store-front -o jsonpath='{.status.loadBalancer.ingress[0].ip}'
```

Or:

```bash
kubectl get svc store-front --output jsonpath='{.status.loadBalancer.ingress[0].ip}' && echo
```

3. You can also ask:

```
@terminal Give me the full URL to access my application
```

Copilot might provide:

```bash
echo "http://$(kubectl get service store-front -o jsonpath='{.status.loadBalancer.ingress[0].ip}')"
```

</details>

<details>
<summary><b>Step 4: Access the Application</b></summary>

1. Copy the URL from the previous step.
2. Open a web browser.
3. Paste the URL: `http://<EXTERNAL-IP>`
4. You should see the **AKS Store Demo** application homepage.

![Application Homepage](The storefront should show a product catalog with pet supplies)

5. Test the application:
   - Browse products
   - Add items to cart
   - Place an order
   - Verify the order appears in the order service

</details>

<details>
<summary><b>Step 5: Verify Backend Services using GitHub Copilot</b></summary>

1. Ask Copilot to help you test the backend APIs:

```
@terminal How can I test if my order-service API health endpoint is working?
```

2. Copilot might suggest port-forwarding:

```bash
kubectl port-forward service/order-service 3000:3000
```

3. In another terminal, test the health endpoints:

```bash
# Test order-service health
curl http://localhost:3000/health

# Test product-service health (in a new terminal)
kubectl port-forward service/product-service 3002:3002
curl http://localhost:3002/health
```

4. Ask Copilot to test RabbitMQ connectivity:

```
@terminal Show me how to verify that order-service can connect to RabbitMQ
```

Example commands:

```bash
# List pods
kubectl get pods

# Exec into order-service pod
kubectl exec -it deployment/order-service -- sh

# Inside the pod, test RabbitMQ connectivity
nc -zv rabbitmq 5672
echo "Testing AMQP port"
nc -zv rabbitmq 15672
echo "Testing Management UI port"
```

</details>

### Summary
✅ Retrieved application external IP  
✅ Accessed the web application  
✅ Verified backend services are working  
✅ Application is fully functional  

---