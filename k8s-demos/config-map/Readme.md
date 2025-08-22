
# Kubernetes ConfigMap Demo

## Introduction

A **ConfigMap** in Kubernetes is an API object used to store non-confidential data in key-value pairs. ConfigMaps allow you to decouple environment-specific configuration from your container images, making your applications more portable and easier to manage.

Typical use cases include storing database connection strings, API endpoints, feature flags, and other configuration data. A single ConfigMap can be shared by multiple pods.

---

## Demo Files

### 1. config1.yml (ConfigMap)
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
	name: config1
data:
	SQL_CONNECTION_STRING: "Server=myServerAddress;Database=myDataBase;User Id=myUsername;Password=myPassword;"
	REDIS_CONNECTION_STRING: "redis:6379"
	API_URL: "http://api.myapp.com"
```

### 2. cm-pod.yml (Pod using ConfigMap)
```yaml
apiVersion: v1
kind: Pod
metadata:
	name: cm-pod
	labels:
		app.kubernetes.io/name: cm-pod
spec:
	containers:
	- name: cm-pod
		image: mahendrshinde/myweb:1
		envFrom:
		- configMapRef:
				name: config1
		resources:
			limits:
				memory: "128Mi"
				cpu: "500m"
		ports:
			- containerPort: 80
```

---

## Step-by-Step Demo

1. **Create the ConfigMap**
	 ```sh
	 kubectl apply -f config1.yml
	 ```
	 Verify:
	 ```sh
	 kubectl describe cm config1
	 ```

2. **Create the Pod that uses the ConfigMap**
	 ```sh
	 kubectl apply -f cm-pod.yml
	 ```

3. **Access the Pod and check environment variables**
	 ```sh
	 kubectl exec -it cm-pod -- sh
	 echo $SQL_CONNECTION_STRING
	 echo $REDIS_CONNECTION_STRING
	 echo $API_URL
	 exit
	 ```

---

## Explanation

- The `config1.yml` file defines a ConfigMap with three key-value pairs.
- The `cm-pod.yml` file creates a Pod that loads all key-value pairs from the ConfigMap as environment variables.
- When you exec into the pod, you can access these variables using standard shell commands.

---

## Clean Up

To remove the resources after the demo:
```sh
kubectl delete -f cm-pod.yml
kubectl delete -f config1.yml
```
