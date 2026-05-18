
# Kubernetes Labels, Selectors, and Annotations Demo

This README contains useful `kubectl` commands for working with Pods, labels, selectors, and annotations.

---

## 🟢 Pod Creation

```bash
kubectl run backend-pod --image=nginx:1.25 -l env=production,app=python
```

---

## 🔍 Pod Details
```bash
kubectl describe po backend-pod
```

---

## 🏷️ Labeling Pods
```bash
kubectl label pod backend-pod version=1.4.2
kubectl label pod backend-pod team=dev tier=backend
```

### Show labels in output
```bash
kubectl get pods --show-labels
kubectl get pods --label-columns app,env,team
kubectl get pods -L app,env,team
```

### Overwrite an existing label
```bash
kubectl label pod backend-pod env=staging --overwrite
```

### Remove a label
```bash
kubectl label pod backend-pod version-
```

---

## 🎯 Using Selectors
```bash
kubectl get pods --selector app=web
kubectl get pods -l app=web,env=production
kubectl get pods -l env!=staging
kubectl get pods -l 'env in (production,staging)'
kubectl get pods -l 'env notin (production)'
kubectl get pods -l 'app'        # key exists
kubectl get pods -l '!version'   # key NOT exist
```

### Delete Pods by selector
```bash
kubectl delete pods -l app=web
```

---

## 📝 Annotations
```bash
kubectl annotate pod my-annotation-pod owner=suhas team=dev
kubectl describe po my-annotation-pod
kubectl annotate pod my-annotation-pod team=UI --overwrite
```

### Remove an annotation
```bash
kubectl annotate pod my-annotation-pod team-
```

---

## 📌 Notes
- **Labels** are key/value pairs used for grouping and selecting resources.  
- **Selectors** filter resources based on labels.  
- **Annotations** store metadata (not used for selection).  
