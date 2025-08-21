## Kubernetes ReplicaSet: Concepts & Demo

### What is a ReplicaSet?
A **ReplicaSet** is a Kubernetes controller that ensures a specified number of pod replicas are running at any given time. If a pod fails or is deleted, the ReplicaSet automatically creates a new one to maintain the desired count. It is commonly used to guarantee high availability and scalability of stateless applications.

---

### Demo ReplicaSet: `myapp.yml` Explained

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp
  labels:
    app: myapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
        - name: myapp
          image: mahendrshinde/myweb:1
          ports:
            - containerPort: 80
```

**Key Points:**
- **replicas: 3** — Ensures 3 pods are always running.
- **selector.matchLabels** — Matches pods with label `app: myapp`.
- **template** — Defines the pod template to use for new pods.
- **image** — Uses the Docker image `mahendrshinde/myweb:1`.

---

## Step-by-Step: Deploy, Scale, and Self-Heal with ReplicaSet

### 1. Deploy the ReplicaSet
```sh
kubectl apply -f myapp.yml
```
Check status:
```sh
kubectl get replicaset
kubectl get pods -l app=myapp
```
**Check system events:**
```sh
kubectl get events --sort-by=.metadata.creationTimestamp
```

### 2. Scale the ReplicaSet
Edit `myapp.yml` and change `replicas: 3` to desired count (e.g., 5), then:
```sh
kubectl apply -f myapp.yml
```
Or scale directly:
```sh
kubectl scale rs myapp --replicas=5
```
Check pods and events:
```sh
kubectl get pods -l app=myapp
kubectl get events --sort-by=.metadata.creationTimestamp
```

### 3. Self-Healing Demo
Delete a pod:
```sh
kubectl delete pod <pod-name>
```
ReplicaSet will automatically create a new pod to maintain the replica count.

Check pods and events:
```sh
kubectl get pods -l app=myapp
kubectl get events --sort-by=.metadata.creationTimestamp
```

---

## Summary
- **ReplicaSet** ensures the desired number of pod replicas are always running.
- Use `kubectl get events` frequently to monitor system activity and troubleshooting.
