# Kubernetes   

## Grundlagen
- Pod: Besteht aus einem oder mehreren Containern die sich den Netzwerk-Namespace teilen. 

## Pod
- YAML:
```yaml
---
# Header
apiVersion: v1
kind: Pod
# Informationen über die Resource
metadata: 
	name: nginx
# Spezifikation der Ressource
spec:
	containers:
	- name: nginx
	  image: nginx:1.19
```
- Pod starten:
```bash
kubectl apply -f pod.yaml
```
- Pods anzeigen: 
```bash
kubectl get pods
```
- Informationen zu einem Pod ausgeben:
```bash
kubectl describe pod nginx
```
- Pod löschen:
```bash
kubectl delete pod nginx
```

## Labels
