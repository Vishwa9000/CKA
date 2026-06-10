# 🚀 Advanced kubectl Commands

This guide covers advanced `kubectl` usage for inspecting, creating, editing, and managing Kubernetes resources without always writing YAML files.

---

## 🔍 Inspecting Resources

### Get Pods with extra details
```bash
kubectl get pods -o wide
```

### Get Pod details in JSON
```bash
kubectl get pod my-pod -o json
```

### Custom columns (name + status)
```bash
kubectl get pods -o custom-columns=NAME:.metadata.name,STATUS:.status.phase
```

### Get Deployment in YAML
```bash
kubectl get deploy my-app -o yaml
```

### Get all Pod names
```bash
kubectl get pods -o jsonpath='{.items[*].metadata.name}'
```

### Get first Pod name
```bash
kubectl get pods -o jsonpath='{.items[0].metadata.name}'
```

### Sort Pods by creation time
```bash
kubectl get pods --sort-by=.metadata.creationTimestamp
```

---

## ⚡ Create Resources Without YAML

### Run a Pod
```bash
kubectl run nginx --image=nginx --port=80
```

### Create a Deployment
```bash
kubectl create deployment web --image=nginx --replicas=3
```

### Expose Deployment as NodePort Service
```bash
kubectl expose deploy web --port=80 --type=NodePort
```

### Create ConfigMap
```bash
kubectl create configmap app-cfg --from-literal=key=val
```

### Create Secret
```bash
kubectl create secret generic db-secret --from-literal=pass=abc123
```

---

## 📝 Editing & Patching

### Edit Deployment in default editor
```bash
kubectl edit deploy web
```
👉 Example: add label `version:v1`

### Patch inline (no editor)
```bash
kubectl patch deploy web -p '{"spec":{"replicas":5}}'
```

### Set image without editing YAML
```bash
kubectl set image deploy/web nginx=nginx:1.25
```

### Set resource limits
```bash
kubectl set resources deploy/web -c nginx --limits=cpu=200m,memory=128Mi
```

---

## 📄 Generate YAML Without Creating

### Pod YAML
```bash
kubectl run nginx --image=nginx --dry-run=client -o yaml > pod.yaml
```

### Deployment YAML
```bash
kubectl create deploy web --image=nginx --dry-run=client -o yaml
```

### Service YAML
```bash
kubectl create svc clusterip my-svc --tcp=80:80 --dry-run=client -o yaml
```

---

## 📚 Understanding API & Specs

### Explain fields
```bash
kubectl explain pod.spec.containers
kubectl explain pod.spec.containers --recursive
```

### API resources & versions
```bash
kubectl api-resources
kubectl api-versions
```

### Find short names
```bash
kubectl api-resources --namespaced=true
```
👉 Example: `ns = namespaces`, `po = pods`

---

## ⚙️ Aliases & Shortcuts

### Short alias for kubectl
```bash
alias k=kubectl
```

### Dry-run shortcut
```bash
export do="--dry-run=client -o yaml"
export now="--force --grace-period 0"
```

Usage:
```bash
k run nginx --image=nginx $do > pod.yaml
k delete pod nginx $now
```

---

## 🏷️ Namespace Management

### Set default namespace
```bash
kubectl config set-context --current --namespace=dev
```

### Quick namespace alias
```bash
alias kn='kubectl config set-context --current --namespace'
```

Switch to production namespace:
```bash
kn production
```

---

## 🔧 Useful One-Liners

### Get events sorted by time
```bash
k get events --sort-by=.lastTimestamp
```

### Watch Pods in real-time
```bash
k get pods -w
```

### Get all resources in a namespace
```bash
k get all -n kube-system
```

---

## 🧠 Key Takeaways
- **`-o wide/json/yaml/custom-columns/jsonpath`** → Different output formats.  
- **`--sort-by`** → Sort results by a chosen field.  
- **`run/create/expose`** → Create resources quickly without YAML.  
- **`edit/patch/set`** → Modify resources inline.  
- **`dry-run`** → Generate YAML without creating resources.  
- **`explain/api-resources/api-versions`** → Explore Kubernetes API.  
- **Aliases** → Save time with shortcuts.  
