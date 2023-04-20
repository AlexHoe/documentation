# Kubernetes   

## Basics
- Pod: One or more containers sharing the network namespace. 

## Pod
- YAML:
```yaml
---
# Header
apiVersion: v1
kind: Pod
# information about the resource
metadata: 
	name: nginx
# specifications of of the resource 
spec:
	containers:
	- name: nginx
	  image: nginx:1.19
```
- start pod:
```bash
kubectl apply -f pod.yaml
```
- show pods: 
```bash
kubectl get pods
```
- Show information of a pod:
```bash
kubectl describe pod nginx
```
- delete pod:
```bash
kubectl delete pod nginx
```

## Labels
