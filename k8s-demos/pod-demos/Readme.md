# Kubernetes Pod Demo

This guide demonstrates how to create, manage, and understand the Quality of Service (QoS) of Kubernetes Pods.

## 1. Creating a Pod

Create a file named `my-pod.yaml` with the following contents:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: web
      image: nginx:latest
```

## 2. Deploying and Managing the Pod

Use the terminal (Command Prompt or PowerShell) to run these commands:

```sh
# Deploy the pod
kubectl apply -f my-pod.yaml

# Check the latest events in the cluster
kubectl get events

# List all pods
kubectl get pods

# Delete the pod
kubectl delete -f my-pod.yaml
```

## 3. Port Forwarding

To access the pod's web server locally, use port forwarding:

```sh
kubectl port-forward pod/my-pod 9090:80
# Press CTRL+C to stop port forwarding
```

Now, you can access the nginx web server at `http://localhost:9090`.

## 4. Pod Quality of Service (QoS) Classes

Kubernetes assigns a QoS class to each pod based on its resource requests and limits:

- **BestEffort**: No resource requests or limits specified.
- **Burstable**: Requests and limits are set, but not equal.
- **Guaranteed**: Requests and limits are set and equal for all resources.

### Burstable Pod Example

A pod is classified as Burstable if it has resource requests and/or limits, but they are not equal.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: web
      image: nginx:latest
      ports:
        - containerPort: 80
      resources:
        limits:
          memory: "128Mi"
          cpu: "500m"
        requests:
          memory: "64Mi"
          cpu: "250m"
```

### Guaranteed Pod Example

A pod is classified as Guaranteed if resource requests and limits are set and equal for all containers.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: web
      image: nginx:latest
      ports:
        - containerPort: 80
      resources:
        limits:
          memory: "64Mi"
          cpu: "250m"
        requests:
          memory: "64Mi"
          cpu: "250m"
```

---
For more details, refer to the [Kubernetes documentation on Pod QoS](https://kubernetes.io/docs/tasks/configure-pod-container/quality-service-pod/).