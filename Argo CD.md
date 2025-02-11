# Argo CD Installation Guide

Argo CD is a declarative, GitOps continuous delivery tool for Kubernetes. Follow these steps to install and configure Argo CD on your cluster.

---

## **Step 1: Install Argo CD in the `argocd` Namespace**
```sh
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
This command installs Argo CD and all its components in the `argocd` namespace.

---

## **Step 2: Verify the Installation**
```sh
kubectl get pods -n argocd
```
Ensure all pods are in the `Running` state before proceeding.

---

## **Step 3: Expose Argo CD Server**

### **Option 1: Use Port Forwarding (For Local Access)**
```sh
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
Now, you can access Argo CD via **https://localhost:8080**

### **Option 2: Expose via a LoadBalancer**
```sh
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```
Retrieve the external IP:
```sh
kubectl get svc argocd-server -n argocd
```

### **Option 3: Expose via an Ingress (Recommended for Production)**
If you're using an ingress controller, create an ingress rule for Argo CD.

---

## **Step 4: Retrieve the Argo CD Admin Password**
By default, Argo CD generates an admin password stored in a Kubernetes secret. Run:
```sh
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 --decode
```
Use **`admin`** as the username and the decoded password to log in.

---

## **Step 5: Login to Argo CD CLI**
```sh
argocd login <ARGOCD_SERVER>
```
Example:
```sh
argocd login localhost:8080 --username admin --password <your_password>
```

---

## **Step 6: Deploy an Application using Argo CD**

### **1. Register a Kubernetes Cluster**
```sh
argocd cluster add <your-cluster-name>
```

### **2. Deploy an App from Git**
```sh
argocd app create my-app \
  --repo https://github.com/example/repo.git \
  --path my-app \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace default
```

### **3. Sync the Application**
```sh
argocd app sync my-app
```

---

## **Step 7: Access Argo CD UI**
- Open your browser and go to:  
  **https://localhost:8080** (or your LoadBalancer IP)
- Log in using the **admin** username and retrieved password.

Now, Argo CD is ready for GitOps-driven Kubernetes deployments! 🚀

