# Kubernetes (K8s) Manifest Files and Commands

## K8s Manifest Files
This project includes the following Kubernetes manifest files:

- `mongo-config.yaml` - This file defines a ConfigMap for MongoDB, which provides configuration settings for the database. The `mongo-url` key in the ConfigMap stores the MongoDB service name (`mongo-service`), allowing other components in the cluster to reference it easily.
- `mongo-secret.yaml` - This file defines a Secret that stores sensitive credentials for MongoDB, including `mongo-user` and `mongo-password`. These credentials are base64 encoded and referenced by the MongoDB and web application deployments for secure authentication.
- `mongo.yaml` - This file contains the deployment and service configuration for MongoDB:
  - **Deployment**: Creates a MongoDB pod using the `mongo:5.0` image and sets environment variables from the Secret.
  - **Service**: Exposes MongoDB within the cluster using a service named `mongo-service`.
- `webapp.yaml` - This file contains the deployment and service configuration for the web application:
  - **Deployment**: Creates a web application pod using the `nanajanashia/k8s-demo-app:v1.0` image and connects to MongoDB using credentials from the Secret and configuration from the ConfigMap.
  - **Service**: Exposes the web application using a `NodePort` service (`webapp-service`), making it accessible on port `30100`.
 
## Configuration Details
### Deployment
```yaml
selector:
  matchLabels:
    app: mongo
```
- Ensures the Deployment manages only the Pods with the label `app: mongo`.

### Service
```yaml
selector:
  app: mongo
```
- Ensures the Service routes traffic only to the Pods labeled `app: mongo`.



## Deployment Process
To deploy the MongoDB and web application components, use the following commands:

1. Apply the MongoDB configuration:
   ```sh
   kubectl apply -f mongo-config.yaml
   ```
   *Creates a ConfigMap named `mongo-config`.*

2. Apply the MongoDB secrets:
   ```sh
   kubectl apply -f mongo-secret.yaml
   ```
   *Creates a Secret named `mongo-secret`.*

3. Deploy MongoDB:
   ```sh
   kubectl apply -f mongo.yaml
   ```
   *Creates MongoDB deployment and service (`mongo-deployment` and `mongo-service`).*

4. Deploy the web application:
   ```sh
   kubectl apply -f webapp.yaml
   ```
   *Creates web application deployment and service (`webapp-deployment` and `webapp-service`).*
   <div style="display: flex; justify-content: center; align-items: center; gap: 10px;">
  <img src="https://github.com/chhsch/k8s_note/blob/5d662076c7119c8a2c84ec8a0c456e79b1bbd03b/images/app.png" alt="Screenshot 1" width="300"/>
</div>


## K8s Commands

### Start Minikube and Check Status
```sh
minikube start --vm-driver=hyperkit 
minikube status
```

### Get Minikube Node's IP Address
```sh
minikube ip
```

### Get Basic Info About K8s Components
```sh
kubectl get node
kubectl get pod
kubectl get svc
kubectl get all
```

### Get Extended Info About Components
```sh
kubectl get pod -o wide
kubectl get node -o wide
```

### Get Detailed Info About a Specific Component
```sh
kubectl describe svc {svc-name}
kubectl describe pod {pod-name}
```

### Get Application Logs
```sh
kubectl logs {pod-name}
```

### Stop Your Minikube Cluster
```sh
minikube stop
```

## Usage Instructions
1. Start Minikube and ensure it's running properly.
2. Deploy the MongoDB and web application manifests using `kubectl apply -f {filename}`.
3. Verify the deployments and services using the commands provided above.
4. Check logs and debug if necessary.
5. Stop Minikube when finished.

## Reference
This project is inspired by the Kubernetes deployment guide from [Kubernetes in 1 Hour](https://gitlab.com/nanuchi/k8s-in-1-hour).

## License
This project is open-source and available under the MIT License.




