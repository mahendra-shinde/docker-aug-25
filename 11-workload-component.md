# Kubernetes Workload Components

Kubernetes provides several workload components to manage and run containerized applications. The main components are:

1. **Pod**
2. **ReplicaSet**
3. **Deployment**

---

## 1. Pod

A **Pod** is the smallest deployable unit in Kubernetes. It can contain one or more containers that share storage, network, and a specification for how to run the containers.

**Example Pod YAML:**
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

---

## 2. ReplicaSet

A **ReplicaSet** ensures that a specified number of pod replicas are running at any given time. If a pod fails or is deleted, the ReplicaSet creates a new one to maintain the desired count.

**Example ReplicaSet YAML:**
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-replicaset
spec:
  replicas: 3
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
```

---

## 3. Deployment

A **Deployment** provides declarative updates for Pods and ReplicaSets. It allows you to easily scale, update, and roll back your applications.

**Example Deployment YAML:**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 3
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
```

---