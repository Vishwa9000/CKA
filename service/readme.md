# Kubernetes Services and DNS Resolution (CoreDNS)

## 📘 Concepts

- **Service**  
  A stable networking abstraction that exposes pods to other components or external clients. Services provide a consistent endpoint (ClusterIP, NodePort, LoadBalancer) regardless of pod IP changes.

- **Types of Services**  
  - **ClusterIP** → Default, accessible only inside the cluster.  
  - **NodePort** → Exposes service on a static port on each node.  
  - **LoadBalancer** → Integrates with cloud provider load balancers for external access.  

- **CoreDNS**  
  Kubernetes’ DNS service that automatically creates DNS records for Services and Pods.  
  - Format: `<service-name>.<namespace>.svc.cluster.local`  
  - Enables service discovery without hardcoding IPs.

---

## ⚙️ Common Service Commands

### 🔹 Expose a Deployment (ClusterIP by default)
```bash
kubectl expose deploy demo --port=80 --target-port=9090
```

### 🔹 Expose a Deployment as NodePort
```bash
kubectl expose deploy demo --port=80 --target-port=9090 --type=NodePort
```

### 🔹 List Services
```bash
kubectl get svc
kubectl get svc -A -o wide
```

### 🔹 Describe a Service
```bash
kubectl describe svc demo
```

### 🔹 Delete a Service
```bash
kubectl delete svc demo
```

### 🔹 View Endpoints
```bash
kubectl get endpoints frontend-svc
```

---

## ⚙️ DNS Resolution with CoreDNS

### 🔹 Test DNS Resolution Inside a Pod
```bash
kubectl exec -it demo-664c99689d-fqkc4 -- curl http://backend-svc.default.svc.cluster.local:8080
```

### 🔹 Check Pod DNS Config
```bash
kubectl exec -it <pod-name> -- cat /etc/resolv.conf
```

### 🔹 Debug DNS Resolution
```bash
kubectl exec -it <pod-name> -- nslookup backend-svc.default.svc.cluster.local
kubectl exec -it <pod-name> -- dig backend-svc.default.svc.cluster.local
```

---

## ➕ Additional Useful Commands

- **List Endpoints for All Services**
```bash
kubectl get endpoints -A
```

- **Check Service IPs**
```bash
kubectl get svc demo -o wide
```

- **Port Forward to Access Service**
```bash
kubectl port-forward svc/demo 8080:80
```

- **Check CoreDNS Logs**
```bash
kubectl logs -n kube-system -l k8s-app=kube-dns
```

---

## 🔑 Key Takeaways
- Services provide stable networking for pods.  
- CoreDNS enables DNS-based service discovery (`<service>.<namespace>.svc.cluster.local`).  
- Use `kubectl expose` to quickly create services.  
- Use `kubectl exec` with `curl`, `nslookup`, or `dig` to test DNS resolution.  
- NodePort and LoadBalancer types allow external access.  

---
