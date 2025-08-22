## Kubernetes Service Demo: Exposing a Deployment

This demo shows how to expose a Kubernetes Deployment using a Service. You will deploy a sample web application and access it via a Kubernetes Service.

### Files in this folder

- `deploy.yml`: Deployment manifest for the sample app (`myapp3`).
- `app-service.yml`: Service manifest to expose the deployment.
- `test-pod.yml`: Pod manifest for testing connectivity inside the cluster.

---

### Steps to Run the Demo

1. **Apply the Deployment**
	```sh
	kubectl apply -f deploy.yml
	```
	This creates a deployment named `myapp3` with 3 replicas.

2. **Check Deployment Status**
	```sh
	kubectl get deployments
	kubectl get pods -o wide
	```

3. **Expose the Deployment with a Service**
	```sh
	kubectl apply -f app-service.yml
	```
	This creates a Service named `myweb` that forwards traffic to pods with label `app: myapp3` on port 80.

4. **Verify the Service**
	```sh
	kubectl get services
	```
	Note the `CLUSTER-IP` assigned to the service.

5. **Test Service Connectivity from Inside the Cluster**
	- Deploy a test pod:
	  ```sh
	  kubectl apply -f test-pod.yml
	  ```
	- Get a shell inside the test pod:
	  ```sh
	  kubectl exec -it test -- sh
	  ```
	- Inside the pod, use `wget` or `curl` to access the service:

	  ```sh
	  curl http://<CLUSTER-IP>
      ## OR USE SERVICE NAME
      curl http://myweb
	  ```
	- You should see the web page served by the app.

6. **Clean Up**
	```sh
	kubectl delete -f test-pod.yml
	kubectl delete -f app-service.yml
	kubectl delete -f deploy.yml
	```

---

### Notes
- The service type is `ClusterIP` (default), so it is only accessible within the cluster.
- For external access, you can use `NodePort` or `LoadBalancer` type services (not covered in this demo).

---

## Exposing the App Externally: NodePort Service

The previous steps used a `ClusterIP` service, which is only accessible within the Kubernetes cluster. To allow access from outside the cluster (e.g., from your browser or another machine on the network), you can use a `NodePort` service.

### What is a NodePort Service?
A NodePort service exposes the application on a static port (the NodePort) on each node's IP address. You can access the app using `<NodeIP>:<NodePort>`.

### Steps to Switch to NodePort Service

1. **Remove the Old Service**
	```sh
	kubectl delete -f app-service.yml
	```
	This deletes the old `ClusterIP` service named `myweb`.

2. **Deploy the NodePort Service**
	```sh
	kubectl apply -f app-nodeport.yml
	```
	This creates a new service named `myweb2` of type `NodePort` that exposes the same deployment (`myapp3`).

3. **Find the NodePort and Access the App**
	```sh
	kubectl get services
	```
	Look for the `myweb2` service and note the `NODE-PORT` (e.g., 30000-32767).

	- Get your node's IP address (for Docker desktop, use `kubernetes.docker.internal` instead of node-ip).

	- Access the app in your browser or with curl:
	  ```sh
	  curl http://<NodeIP>:<NodePort>
	  ```

    > for Docker desktop use URL http://kubernetes.docker.internal:<NODEPORT>

4. **(Optional) Clean Up**
	```sh
	kubectl delete -f app-nodeport.yml
	```

---
## Exposing the App with a LoadBalancer Service

For cloud environments (or local environments that support it), you can use a `LoadBalancer` service to expose your app externally. This type of service provisions an external IP (or hostname) and load balances traffic to your pods.

### What is a LoadBalancer Service?
A LoadBalancer service automatically creates an external load balancer (if supported by your Kubernetes environment) and assigns a public IP address. This is the most common way to expose services in cloud providers like AWS, Azure, or GCP.

### Steps to Use LoadBalancer Service

1. **Remove the Previous Service (if any)**
	```sh
	kubectl delete -f app-nodeport.yml
	```
	This deletes the NodePort service if it exists.

2. **Deploy the LoadBalancer Service**
	```sh
	kubectl apply -f app-lb.yml
	```
	This creates a service named `myweb2` of type `LoadBalancer` that exposes the same deployment (`myapp3`).

3. **Find the External IP and Access the App**
	```sh
	kubectl get services
	```
	Wait for the `EXTERNAL-IP` to be assigned to the `myweb2` service. This may take a few minutes in cloud environments.

	- Access the app in your browser or with curl:
	  ```sh
	  curl http://<EXTERNAL-IP>:8050
	  ```
	- For local environments (like Docker Desktop), you may need to use `kubernetes.docker.internal` or check your environment's documentation for accessing LoadBalancer services.

4. **(Optional) Clean Up**
	```sh
	kubectl delete -f app-lb.yml
	```

