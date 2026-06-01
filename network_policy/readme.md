# 🔒 Kubernetes NetworkPolicy Basic Commands

```bash
# Step 1: Create a namespace for testing
kubectl create namespace netpol-demo
```

```bash
# Step 2: Deploy two sample pods
kubectl run pod-a --image=nginx -n netpol-demo
kubectl run pod-b --image=nginx -n netpol-demo
```

```bash
# Step 3: Expose pod-a with a service
kubectl expose pod pod-a --port=80 --name=svc-a -n netpol-demo
```

```bash
# Step 4: Create a default deny-all policy
kubectl apply -n netpol-demo -f - <<EOF
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all
spec:
  podSelector: {}
  policyTypes:
  - Ingress
EOF
```

```bash
# Step 5: Allow traffic only from pod-b to pod-a
kubectl apply -n netpol-demo -f - <<EOF
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-pod-b-to-a
spec:
  podSelector:
    matchLabels:
      run: pod-a
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          run: pod-b
EOF
```

```bash
# Step 6: Verify policies
kubectl get networkpolicy -n netpol-demo
kubectl describe networkpolicy deny-all -n netpol-demo
kubectl describe networkpolicy allow-pod-b-to-a -n netpol-demo
```

```bash
# Step 7: Test connectivity
kubectl exec -it pod-b -n netpol-demo -- curl svc-a:80   # should work
kubectl exec -it pod-a -n netpol-demo -- curl svc-a:80   # self-access
kubectl run pod-c --image=nginx -n netpol-demo --restart=Never
kubectl exec -it pod-c -n netpol-demo -- curl svc-a:80   # should be blocked
```
