# Kubernetes JSONPath and Sorting Examples

This README demonstrates useful `kubectl` commands for working with Pods and Nodes, focusing on JSONPath queries and sorting.

---

## 🟢 Create a Pod
```bash
kubectl run my-pod --image=nginx:latest
```

---

## 🔍 Get Pod details in JSON
```bash
kubectl get pod my-pod -o json
```

---

## 📚 JSONPath Examples

### Get Pod name (explicit root)
```bash
kubectl get pod my-pod -o jsonpath='{$.metadata.name}'
```

### Get Pod name (implicit root)
```bash
kubectl get pod my-pod -o jsonpath='{.metadata.name}'
```

### Get Pod IP
```bash
kubectl get pod my-pod -o jsonpath='{.status.podIP}'
```

### Get Pod image
```bash
kubectl get pod my-pod -o jsonpath='{.status.containerStatuses[0].image}'
```

### Get all Pod names
```bash
kubectl get pods -o jsonpath='{.items[*].metadata.name}'
```

### Get first Pod name
```bash
kubectl get pods -o jsonpath='{.items[0].metadata.name}'
```

### Get Pod names with IPs (row by row)
```bash
kubectl get pods -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.status.podIP}{"\n"}{end}'
```

### Alternative (names and IPs in separate lists)
```bash
kubectl get pods -o jsonpath='{.items[*].metadata.name}{"\t"}{.items[*].status.podIP}'
```

- **With `range`** → Think of it like a **for loop**: “For each Pod, print name and IP together.”
- **Without `range`** → It’s like asking: “Give me all names in one list, and all IPs in another list.” They don’t match up row by row.

---

## 🎯 Filtering Pods

### Get names of Pods that are Running
```bash
kubectl get pods -o jsonpath='{.items[?(@.status.phase=="Running")].metadata.name}'
```

- **`?()`** → Filter expression: include items that match the condition.  
- **`@`** → Refers to the current item in the list.  
- **`.status.phase`** → Field being checked (Pod lifecycle phase).  
- **`=="Running"`** → Condition: only keep Pods where phase equals Running.

---

## 📊 Node Information

### Tab-separated Node names and kubelet versions
```bash
kubectl get nodes -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.status.nodeInfo.kubeletVersion}{"\n"}{end}'
```

---

## 🔄 Sorting Examples

### Sort Pods by creation time
```bash
kubectl get pods --sort-by=.metadata.creationTimestamp
```
👉 List all Pods, sorted by the time they were created (oldest first).

### Sort Pods by restart count
```bash
kubectl get pods --sort-by=.status.containerStatuses[0].restartCount
```

- **`kubectl get pods`** → List all Pods.  
- **`--sort-by=...`** → Arrange the list based on a particular field.  
- **`.status.containerStatuses[0].restartCount`** →  
  - `.status.containerStatuses` → List of container statuses inside each Pod.  
  - `[0]` → First container in the Pod.  
  - `.restartCount` → How many times that container has restarted.

---

## 🧠 Key Takeaways
- **`-o jsonpath`** → Extract specific fields from Kubernetes objects.  
- **`range`** → Loop through items one by one.  
- **`?(@...)`** → Filter items based on conditions.  
- **`--sort-by`** → Sort results by a chosen field.  
```
