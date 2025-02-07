**list of `kubectl` commands**  


# `kubectl` Commands Cheat Sheet

`kubectl` is the command-line tool used to interact with Kubernetes clusters.



## 1. Cluster Information
```bash
kubectl cluster-info
kubectl get nodes
kubectl get componentstatuses
kubectl config view
```

---

## 2. Working with Pods

### List Pods
```bash
kubectl get pods
kubectl get pods -o wide
kubectl get pods -n <namespace>
```

### Describe a Pod
```bash
kubectl describe pod <pod-name>
```

### Delete a Pod
```bash
kubectl delete pod <pod-name>
kubectl delete pod --all
```

### Get Pod Logs
```bash
kubectl logs <pod-name>
kubectl logs -f <pod-name>   # Follow logs (like tail -f)
kubectl logs <pod-name> -c <container-name>   # Get logs of a specific container in a pod
```

### Execute a Command Inside a Pod
```bash
kubectl exec -it <pod-name> -- /bin/bash   # Open a shell in a pod
kubectl exec -it <pod-name> -- ls -al      # Run a specific command
```

---

## 3. Working with Deployments

### List Deployments
```bash
kubectl get deployments
kubectl get deployments -o wide
```

### Describe a Deployment
```bash
kubectl describe deployment <deployment-name>
```

### Scale a Deployment
```bash
kubectl scale deployment <deployment-name> --replicas=3
```

### Rollout Commands
```bash
kubectl rollout status deployment <deployment-name>
kubectl rollout history deployment <deployment-name>
kubectl rollout undo deployment <deployment-name>   # Rollback to the previous version
```

### Delete a Deployment
```bash
kubectl delete deployment <deployment-name>
```

---

## 4. Working with Services

### List Services
```bash
kubectl get services
kubectl get svc -o wide
```

### Describe a Service
```bash
kubectl describe service <service-name>
```

### Expose a Deployment as a Service
```bash
kubectl expose deployment <deployment-name> --type=LoadBalancer --port=80 --target-port=8080
```

### Delete a Service
```bash
kubectl delete service <service-name>
```

---

## 5. Working with Namespaces

### List Namespaces
```bash
kubectl get namespaces
```

### Create a Namespace
```bash
kubectl create namespace <namespace-name>
```

### Delete a Namespace
```bash
kubectl delete namespace <namespace-name>
```

### Run a Command in a Specific Namespace
```bash
kubectl get pods -n <namespace>
```

---

## 6. Working with ConfigMaps & Secrets

### List ConfigMaps
```bash
kubectl get configmaps
```

### Create a ConfigMap
```bash
kubectl create configmap <config-name> --from-literal=key=value
kubectl create configmap <config-name> --from-file=config.yaml
```

### List Secrets
```bash
kubectl get secrets
```

### Create a Secret
```bash
kubectl create secret generic <secret-name> --from-literal=username=admin --from-literal=password=secret
kubectl create secret generic <secret-name> --from-file=secret.txt
```

---

## 7. Working with Persistent Volumes

### List Persistent Volumes (PV) and Persistent Volume Claims (PVC)
```bash
kubectl get pv
kubectl get pvc
```

### Describe a Persistent Volume
```bash
kubectl describe pv <pv-name>
```

---

## 8. Working with Ingress

### List Ingress Rules
```bash
kubectl get ingress
```

### Describe an Ingress
```bash
kubectl describe ingress <ingress-name>
```

### Apply an Ingress YAML Configuration
```bash
kubectl apply -f ingress.yaml
```

---

## 9. Apply & Delete Kubernetes Manifests

### Apply a YAML Configuration
```bash
kubectl apply -f <filename>.yaml
kubectl apply -f <directory>/
```

### Delete a Resource from YAML
```bash
kubectl delete -f <filename>.yaml
```

---

## 10. Debugging & Troubleshooting

### Get Events
```bash
kubectl get events --sort-by=.metadata.creationTimestamp
```

### Debug a Pod
```bash
kubectl debug pod/<pod-name> --image=busybox -it
```

### Get All Resources
```bash
kubectl get all
```

### View Logs
```bash
kubectl logs <pod-name> -f
```

### Check Running Resources in a Namespace
```bash
kubectl get all -n <namespace>
```

---

## 11. Get Help

```bash
kubectl help
kubectl <command> --help
```

---

## Conclusion

These commands cover the most commonly used Kubernetes operations. For more details, visit the official Kubernetes documentation:  
[Kubernetes Documentation](https://kubernetes.io/docs/)
