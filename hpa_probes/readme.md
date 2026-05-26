# Kubernetes HPA and Probes

## 📘 Concepts

- **Horizontal Pod Autoscaler (HPA)**  
  Automatically scales the number of pod replicas in a Deployment, ReplicaSet, or StatefulSet based on observed CPU/memory utilization or custom metrics.

- **Probes**  
  Mechanisms for the kubelet to check container health:
  - **Startup probe** → Ensures the container has finished booting before other probes run.  
  - **Readiness probe** → Determines if the pod is ready to serve traffic.  
  - **Liveness probe** → Detects if the container is still alive; restarts it if not.

---

## ⚙️ Common HPA Commands

### 🔹 Create HPA (CPU-based)
```bash
kubectl autoscale deployment my-app --cpu-percent=50 --min=2 --max=10
```

### 🔹 View HPA
```bash
kubectl get hpa
kubectl describe hpa my-app
```

### 🔹 Delete HPA
```bash
kubectl delete hpa my-app
```

### 🔹 Check Metrics
```bash
kubectl top pod
kubectl top node
```

---

## ⚙️ Common Probe Commands

### 🔹 Check Pod Status
```bash
kubectl get pods
kubectl describe pod <pod-name>
```

### 🔹 View Probe Events
```bash
kubectl logs <pod-name>
kubectl describe pod <pod-name> | grep -i probe
```

### 🔹 Debugging Probe Failures
```bash
kubectl get events --sort-by=.metadata.creationTimestamp
kubectl exec -it <pod-name> -- curl http://localhost:80/
```

---

## 🔑 Key Takeaways
- **HPA** keeps workloads efficient by scaling pods up/down automatically.  
- **Startup probe** prevents premature restarts during initialization.  
- **Readiness probe** controls traffic routing (pod removed from Service endpoints if not ready).  
- **Liveness probe** ensures unhealthy containers are restarted.  
- Use `kubectl top` to monitor resource usage and verify autoscaling triggers.  
- Use `kubectl describe` and `kubectl logs` to debug probe issues.  
