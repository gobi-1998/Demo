# Demo
Q5.
To deploy an Nginx application on your Kubernetes cluster and make it available across the cluster on port 80, you can follow these steps:

1. **Create a Kubernetes Deployment**:

   Create a Kubernetes Deployment resource to manage the Nginx application. You can do this by creating a YAML file, such as `nginx-deployment.yaml`, with the following content:

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: nginx-deployment
   spec:
     replicas: 3  # You can adjust the number of replicas as needed
     selector:
       matchLabels:
         app: nginx
     template:
       metadata:
         labels:
           app: nginx
       spec:
         containers:
         - name: nginx
           image: nginx:latest
           ports:
           - containerPort: 80
   ```

   This YAML file defines a Deployment with three replicas of Nginx containers, each listening on port 80.

2. **Apply the Deployment**:

   Apply the Deployment to your Kubernetes cluster using the `kubectl apply` command:

   ```bash
   kubectl apply -f nginx-deployment.yaml
   ```

   This will create the Nginx Deployment and the associated pods.

3. **Create a Kubernetes Service**:

   To make the Nginx application accessible across the cluster on port 80, you can create a Kubernetes Service. Create another YAML file, such as `nginx-service.yaml`, with the following content:

   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: nginx-service
   spec:
     selector:
       app: nginx
     ports:
     - protocol: TCP
       port: 80
       targetPort: 80
     type: LoadBalancer
   ```

   This YAML file defines a Service that exposes the Nginx pods on port 80 and uses a LoadBalancer type to make the service externally accessible.

4. **Apply the Service**:

   Apply the Service to your Kubernetes cluster:

   ```bash
   kubectl apply -f nginx-service.yaml
   ```

   Kubernetes will allocate an external IP (if using a cloud provider) or assign a ClusterIP for the Service to handle traffic on port 80.

5. **Access the Nginx Application**:

   To access the Nginx application across the cluster, you can use the external IP (if available) or the ClusterIP of the Service. Use the following command to check the external IP:

   ```bash
   kubectl get svc nginx-service
   ```

   Open a web browser and navigate to the external IP or the ClusterIP on port 80 to access the Nginx application.

By following these steps, you should have deployed an Nginx application on your Kubernetes cluster, and it should be available across the cluster on port 80. The exact steps may vary depending on your Kubernetes environment and configuration.



Q7


To deploy a web application in a Kubernetes pod, create a replica set, and scale the number of replicas as needed when the load increases, you can follow these steps:

1. **Create a Deployment**:
   
   Start by creating a Deployment that manages your web application. A Deployment is a higher-level abstraction that manages ReplicaSets for you. Create a YAML file, e.g., `web-app-deployment.yaml`, with the following content:

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: web-app-deployment
   spec:
     replicas: 3  # Initially, deploy 3 replicas. Adjust as needed.
     selector:
       matchLabels:
         app: web-app
     template:
       metadata:
         labels:
           app: web-app
       spec:
         containers:
         - name: web-app-container
           image: your-web-app-image:latest
           ports:
           - containerPort: 80
   ```

   Replace `your-web-app-image` with the actual image name of your web application.

2. **Apply the Deployment**:

   Apply the Deployment to create the initial set of pods:

   ```bash
   kubectl apply -f web-app-deployment.yaml
   ```

   This will create three replicas of your web application.

3. **Scale the Deployment**:

   When the load increases and you need more replicas, you can scale the Deployment using the `kubectl scale` command. For example, to increase the number of replicas to 5:

   ```bash
   kubectl scale deployment web-app-deployment --replicas=5
   ```

   This command will adjust the number of replicas to 5 to handle increased load.

With this setup, Kubernetes will manage the desired number of replicas for your web application, and you can easily scale the replicas up or down based on your requirements.
