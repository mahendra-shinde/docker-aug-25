# Kubernetes Deployment Object: Concepts & Demo

## What is a Deployment?
A **Deployment** in Kubernetes is a higher-level controller that manages ReplicaSets and provides declarative updates for Pods and ReplicaSets. It enables you to:
- Deploy stateless applications reliably
- Perform rolling updates and rollbacks
- Scale applications up or down
- Ensure the desired state is always maintained

---

## Demo Deployment: `deploy.yml` Explained

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp3
spec:
  replicas: 3
  strategy:
    type: Recreate
  selector:
    matchLabels:
      app: myapp3
  template:
    metadata:
      labels:
        app: myapp3
    spec:
      containers:
      - name: myapp
        image: mahendrshinde/myweb:1
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
          requests:
            memory: "64Mi"
            cpu: "250m"
        ports:
        - containerPort: 80
```

**Key Points:**
- **replicas: 3** — Ensures 3 pods are always running.
- **strategy.type: Recreate** — All old pods are killed before new ones are created (useful for non-overlapping updates).
- **selector.matchLabels** — Matches pods with label `app: myapp3`.
- **template** — Defines the pod template for new pods.
- **resources** — Sets CPU and memory requests/limits for each pod.

---

## Step-by-Step: Deploy, Update, Scale, and Rollback

### 1. Deploy the Application
```sh
kubectl apply -f deploy.yml
```
Check status:
```sh
kubectl get deployments
kubectl get pods -l app=myapp3
kubectl get events --sort-by=.metadata.creationTimestamp
```

### 2. Update the Deployment (Rolling Update)
Edit the `image` field in `deploy.yml` (e.g., change to `mahendrshinde/myweb:2`), then:
```sh
kubectl apply -f deploy.yml
```
Monitor rollout:
```sh
kubectl rollout status deployment/myapp3
kubectl get events --sort-by=.metadata.creationTimestamp
```

### 3. Scale the Deployment
```sh
kubectl scale deployment myapp3 --replicas=5
```
Or edit `deploy.yml` and re-apply.
Check pods and events:
```sh
kubectl get pods -l app=myapp3
kubectl get events --sort-by=.metadata.creationTimestamp
```

### 4. Rollback to Previous Version
```sh
kubectl rollout undo deployment/myapp3
```
Check status and events:
```sh
kubectl rollout status deployment/myapp3
kubectl get events --sort-by=.metadata.creationTimestamp
```

---

## Summary
- **Deployment** provides declarative updates, scaling, and rollback for your applications.
- Use `kubectl get events` frequently to monitor system activity and troubleshoot issues.
