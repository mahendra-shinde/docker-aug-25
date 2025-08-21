# Pod Demo

1. Create my-pod.yaml file and use these contents"

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

2. Using Terminal (Command prompt or powershell) use following command to deploy pod

```
kubectl apply -f my-pod.yml
# Check the latest events (happening inside cluster) 
kubectl events
# List all pods
kubectl get pods
# Delete the pod
kubectl delete -f my-pod.yml
```

