# Kubernetes Deployment Rollout and Scaling Guide

This README contains useful `kubectl` commands for managing Deployments, ReplicaSets, Pods, and rolling updates.

---

## 🔍 Inspect Resources
```bash
kubectl get replicasets
kubectl get pods
kubectl describe replicaset my-replicaset
```

---

## 🔄 Trigger a Rolling Update
```bash
kubectl set image deployment/my-app nginx=nginx:1.22
```

---

## 👀 Watch Rollout Progress
```bash
kubectl rollout status deployment/my-app
```

---

## 📜 View Rollout History
```bash
kubectl rollout history deployment/my-app
```

---

## ⏪ Rollback
### Rollback to previous revision
```bash
kubectl rollout undo deployment/my-app
```

### Rollback to a specific revision
```bash
kubectl set image deployment/my-app nginx=nginx:1.23
kubectl rollout undo deployment/my-app --to-revision=2
```

---

## ⏸️ Pause and Resume Rollout
```bash
kubectl rollout pause deployment/my-app
kubectl rollout resume deployment/my-app
```

---

## 📈 Scaling Deployments
### Scale up
```bash
kubectl scale deployment my-app --replicas=5
```

### Scale down
```bash
kubectl scale deployment my-app --replicas=2
```

---

## 🆕 Create a New Deployment
```bash
kubectl create deployment my-app-2 --image=nginx:1.25
```
