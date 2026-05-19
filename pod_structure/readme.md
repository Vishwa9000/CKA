# Kubernetes Pod Inspection and Troubleshooting

This README contains useful `kubectl` commands for inspecting Pods, checking logs, and debugging init containers.

---

## 🔍 Inspect Pod Details
```bash
kubectl get pod <name> -o wide
kubectl describe pod <name>
```

---

## 📜 Logs
```bash
kubectl logs <name>
```

### Logs from specific container
```bash
kubectl logs app-with-sidecar -c log-shipper
```

---

## 🖥️ Exec into a Container
```bash
kubectl exec -it app-with-sidecar -c app -- sh
```

---

## 🛠️ Debugging Init Containers
### Pod stuck in Init phase
```bash
kubectl get pod app-with-init
```

### Describe Pod to see which init is stuck
```bash
kubectl describe pod app-with-init
```

### Logs from init containers
```bash
kubectl logs app-with-init -c wait-for-db
kubectl logs app-with-init -c copy-config
```

---

## 👀 Watch Pod Status Live
```bash
kubectl get pod app-with-init -w
```
