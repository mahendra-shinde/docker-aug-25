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

Happy Learning!
