# Kubernetes Secrets: Concepts and Demo

## What are Kubernetes Secrets?
Kubernetes Secrets are objects used to store sensitive information, such as passwords, OAuth tokens, and SSH keys. Storing such information in a Secret is safer and more flexible than putting it verbatim in a Pod definition or a container image.

### Why use Secrets?
- **Security**: Secrets are encoded (base64) and can be restricted via RBAC.
- **Separation of Concerns**: Keeps sensitive data separate from application code and configuration.
- **Dynamic Updates**: Secrets can be updated independently of Pods.

## Types of Secrets
- **Opaque**: Default type, stores arbitrary user-defined data.
- **docker-registry**: For Docker credentials.
- **tls**: For storing TLS certificates and keys.

## How Secrets Work in Kubernetes
- Secrets are created as Kubernetes objects.
- They can be mounted as files or exposed as environment variables in Pods.
- Pods reference Secrets using `envFrom`, `env`, or `volumes`.

---

# Demo: Using Secrets in a Pod

This demo shows how to create a Secret and use it in a Pod as environment variables.

## 1. Secret Definition (`secret1.yml`)
```yaml
apiVersion: v1
kind: Secret
metadata:
	name: secret1
data:
	SQL_CONNECTION_STRING: "U2VydmVyPW15U2VydmVyQWRkcmVzcztEYXRhYmFzZT1teURhdGFCYXNlO1VzZXIgSWQ9bXlVc2VybmFtZTtQYXNzd29yZD1teVBhc3N3b3JkOwo="
	REDIS_CONNECTION_STRING: "cmVkaXM6NjM3OQ=="
	API_URL: "aHR0cDovL2FwaS5teWFwcC5jb20="
```
- The values are base64-encoded.
- Example: `echo -n 'Server=myServerAddress;Database=myDataBase;User Id=myUsername;Password=myPassword;' | base64`

## 2. Pod Definition (`sc-pod.yml`)
```yaml
apiVersion: v1
kind: Pod
metadata:
	name: scpod
	labels:
		app.kubernetes.io/name: scpod
spec:
	containers:
	- name: scpod
		image: mahendrshinde/myweb:1
		envFrom:
		- secretRef:
				name: secret1
		resources:
			limits:
				memory: "128Mi"
				cpu: "500m"
		ports:
			- containerPort: 80
```
- The `envFrom` field injects all key-value pairs from the Secret as environment variables in the container.

## 3. Steps to Run the Demo

1. **Create the Secret:**
	 ```sh
	 kubectl apply -f secret1.yml
	 ```
2. **Create the Pod:**
	 ```sh
	 kubectl apply -f sc-pod.yml
	 ```
3. **Verify the Pod is Running:**
	 ```sh
	 kubectl get pods
	 ```
4. **Check Environment Variables in the Pod:**
	 ```sh
	 kubectl exec -it scpod -- sh
	 # Inside the pod shell:
	 env | grep CONNECTION_STRING
	 env | grep API_URL
	 ```

## 4. Decoding Secret Values
To view the actual values stored in the Secret:
```sh
kubectl get secret secret1 -o yaml
```
Copy the base64 value and decode it:
```sh
echo 'U2VydmVyPW15U2VydmVyQWRkcmVzcztEYXRhYmFzZT1teURhdGFCYXNlO1VzZXIgSWQ9bXlVc2VybmFtZTtQYXNzd29yZD1teVBhc3N3b3JkOwo=' | base64 -d
```

---

## Best Practices
- Avoid storing plain text secrets in code repositories.
- Use RBAC to restrict access to Secrets.
- Consider using external secret management tools for production (e.g., HashiCorp Vault, AWS Secrets Manager).

---

## References
- [Kubernetes Secrets Documentation](https://kubernetes.io/docs/concepts/configuration/secret/)
- [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
