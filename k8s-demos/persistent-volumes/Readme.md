# Persistent Volumes

## 1. Define Persistent Volumes (PV)
- **Persistent Volumes (PV)** are storage resources in a Kubernetes cluster that exist independently of pods. They allow data to persist beyond the lifecycle of individual pods.
- PVs are provisioned by administrators or dynamically using Storage Classes.

## 2. Define Storage Class and Retain Modes
- **Storage Class**: Defines the type of storage (e.g., SSD, HDD, cloud storage) and the way it is provisioned.

> **Note:**
> In Docker Desktop's built-in Kubernetes, a default StorageClass named `hostpath` (or similar) is usually provided. This allows for dynamic provisioning of persistent volumes without any extra configuration. You can check the default StorageClass using:
> 
> ```sh
> kubectl get storageclass
> ```
> 
> The default StorageClass is used automatically if a PVC does not specify a particular storage class.

- **Reclaim Policy (Retain Modes)**:
	- `Retain`: Keeps the data even after the PVC is deleted.
	- `Delete`: Deletes the storage resource when the PVC is deleted.
	- `Recycle`: (Deprecated) Recycles the volume for future use.

## 3. Define Dynamic Provisioning with PVC and PV
- **Persistent Volume Claim (PVC)**: A request for storage by a user. PVCs are bound to PVs.
- **Dynamic Provisioning**: When a PVC is created, Kubernetes can automatically provision a PV using a Storage Class, eliminating the need for manual PV creation.


## 5. Steps to Try PVC using `claim.yaml` and `vol-pod.yml`

Follow these steps to create and use a Persistent Volume Claim (PVC) in Kubernetes:

1. **Apply the PVC definition:**
	```sh
	kubectl apply -f claim.yaml
	```
	This will create a PersistentVolumeClaim as defined in `claim.yaml`.

2. **Verify the PVC status:**
	```sh
	kubectl get pvc
	```
	Ensure the status is `Bound`, which means the PVC is successfully linked to a Persistent Volume.

3. **Apply the Pod definition:**
	```sh
	kubectl apply -f vol-pod.yml
	```
	This will create a pod that uses the PVC for persistent storage.

4. **Check the Pod status:**
	```sh
	kubectl get pods
	```
	Make sure the pod is running.

5. **(Optional) Inspect the mounted volume:**
	You can exec into the pod to verify the volume is mounted and accessible:
	```sh
	kubectl exec -it vol-pod -- /bin/sh
	```
	