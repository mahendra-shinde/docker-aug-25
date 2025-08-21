# Kubernetes: Introduction, Components, and Architecture

## What is Kubernetes?
Kubernetes (often abbreviated as K8s) is an open-source platform for automating deployment, scaling, and management of containerized applications. It was originally designed by Google and is now maintained by the Cloud Native Computing Foundation (CNCF).

---

## Kubernetes Architecture

Kubernetes follows a master-worker (control plane-node) architecture:

### 1. Control Plane (Master)
Responsible for managing the cluster. Key components:

- **kube-apiserver**: The front-end for the Kubernetes control plane. All communication (kubectl, UI, etc.) goes through the API server.
- **etcd**: A distributed key-value store used for storing all cluster data and configuration.
- **kube-scheduler**: Assigns newly created pods to nodes based on resource requirements and policies.
- **kube-controller-manager**: Runs controllers that handle routine tasks (e.g., node controller, replication controller).
- **cloud-controller-manager**: Integrates with cloud provider APIs (optional, for cloud environments).

### 2. Node (Worker)
Runs the actual application workloads. Key components:

- **kubelet**: An agent that runs on each node and ensures containers are running as expected.
- **kube-proxy**: Maintains network rules for pod communication and service exposure.
- **Container Runtime**: Software responsible for running containers (e.g., Docker, containerd, CRI-O).

---

## Kubernetes Components Overview

| Component                | Description                                                      |
|--------------------------|------------------------------------------------------------------|
| kube-apiserver           | Exposes the Kubernetes API                                       |
| etcd                     | Stores all cluster data                                          |
| kube-scheduler           | Schedules pods to nodes                                          |
| kube-controller-manager  | Runs controllers for cluster state                               |
| kubelet                  | Ensures containers are running on nodes                          |
| kube-proxy               | Handles networking and load balancing                            |
| Container Runtime        | Runs containers (Docker, containerd, etc.)                       |

---

## kubeconfig

`kubeconfig` is a YAML file that contains cluster connection information, user credentials, and context settings. It allows `kubectl` and other clients to authenticate and interact with the Kubernetes cluster.

**Default location:**
```
~/.kube/config
```

**Key sections:**
- `clusters`: Information about Kubernetes clusters
- `users`: Authentication info (certificates, tokens, etc.)
- `contexts`: Which user/cluster combination to use

**Example kubeconfig snippet:**
```yaml
apiVersion: v1
clusters:
	- cluster:
			server: https://1.2.3.4:6443
		name: my-cluster
users:
	- name: my-user
		user:
			token: abcdef123456
contexts:
	- context:
			cluster: my-cluster
			user: my-user
		name: my-context
current-context: my-context
```

---

## kubectl

`kubectl` is the command-line tool for interacting with Kubernetes clusters.

**Common kubectl commands:**

- `kubectl get nodes` — List all nodes in the cluster
- `kubectl get pods` — List all pods in the current namespace
- `kubectl get services` — List all services
- `kubectl describe pod <pod-name>` — Detailed info about a pod
- `kubectl apply -f <file.yaml>` — Create/update resources from a YAML file
- `kubectl logs <pod-name>` — View logs from a pod
- `kubectl exec -it <pod-name> -- /bin/sh` — Execute a shell inside a pod

**kubectl context management:**
- `kubectl config get-contexts` — List available contexts
- `kubectl config use-context <context-name>` — Switch context

---

## Summary

- Kubernetes is a powerful platform for managing containerized applications at scale.
- It uses a master-worker architecture with well-defined components.
- `kubeconfig` files and `kubectl` are essential tools for cluster management.

---

**Next Steps:**
- Explore deploying your first application on Kubernetes.
- Learn about Deployments, Services, and Ingress resources.
- Practice using `kubectl` with a test cluster (e.g., Minikube, Kind, or a cloud provider).
