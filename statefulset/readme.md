# 📘 StatefulSet (STS) Guide

## 🔎 What is a StatefulSet?
A **StatefulSet** manages stateful applications in Kubernetes. It provides:
- Stable, unique Pod identities (`pod-0`, `pod-1`, …)
- Stable storage via `volumeClaimTemplates`
- Ordered deployment, scaling, and deletion
- Required **Headless Service** for DNS identities

---

## ⚙️ Common Commands

### **Create a StatefulSet**
```bash
kubectl apply -f statefulset.yaml
```

### **List StatefulSets**
```bash
kubectl get statefulsets
```

### **Describe a StatefulSet**
```bash
kubectl describe statefulset <sts-name>
```

### **Check Pods created by STS**
```bash
kubectl get pods -l app=<label>
```

### **Delete a StatefulSet (keep PVCs)**
```bash
kubectl delete statefulset <sts-name>
```

### **Delete StatefulSet and PVCs**
```bash
kubectl delete statefulset <sts-name> --cascade=foreground
kubectl delete pvc -l app=<label>
```

---

## 📦 Storage Commands

### **View PVCs created by STS**
```bash
kubectl get pvc
```

Each replica gets its own PVC:
```
data-mongo-0
data-mongo-1
data-mongo-2
```

### **Describe a PVC**
```bash
kubectl describe pvc <pvc-name>
```

---

## 🌐 Networking Commands

### **Check Headless Service**
```bash
kubectl get svc
kubectl describe svc <service-name>
```

### **DNS Identity of Pods**
```
<pod-name>.<service-name>.<namespace>.svc.cluster.local
```

Example:
```
mongo-0.mongo-service.default.svc.cluster.local
mongo-1.mongo-service.default.svc.cluster.local
mongo-2.mongo-service.default.svc.cluster.local
```

---

## 🔄 Scaling Commands

### **Scale up replicas**
```bash
kubectl scale statefulset <sts-name> --replicas=5
```

### **Scale down replicas**
```bash
kubectl scale statefulset <sts-name> --replicas=2
```

---

## 🛠️ Debugging Commands

### **Logs of a Pod**
```bash
kubectl logs <pod-name>
```

### **Exec into a Pod**
```bash
kubectl exec -it <pod-name> -- /bin/bash
```

### **Check Pod DNS resolution**
```bash
kubectl exec -it <pod-name> -- nslookup <other-pod-name>.<service-name>
```
