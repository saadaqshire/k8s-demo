# kubectl Troubleshooting Guide

## Check Pod Status
```bash
kubectl get pods -n dev
kubectl describe pod -n dev 
```

## View Logs
```bash
kubectl logs -n dev 
kubectl logs -n dev  --previous  # Logs from crashed container
kubectl logs -n dev -l app=k8s-demo-app    # All pods with label
```

## Debug Failing Pod
```bash
# 1. Check pod status
kubectl get pods -n dev

# 2. Describe pod for events
kubectl describe pod -n dev 

# 3. Check logs
kubectl logs -n dev 

# 4. Check if image exists
kubectl describe pod -n dev  | grep Image

# 5. Exec into pod to debug
kubectl exec -it -n dev  -- /bin/sh
```

## Common Issues

### Pod stuck in Pending
- **Check:** `kubectl describe pod -n dev <pod-name>`
- **Look for:** "Insufficient cpu" or "Insufficient memory"
- **Fix:** Reduce resource requests or add more nodes

### Pod stuck in ImagePullBackOff
- **Check:** `kubectl describe pod -n dev <pod-name>`
- **Look for:** "Failed to pull image"
- **Fix:** Verify image name and tag are correct

### Pod CrashLoopBackOff
- **Check:** `kubectl logs -n dev <pod-name> --previous`
- **Look for:** Application errors in logs
- **Fix:** Fix application code or configuration

### Health Check Failing
- **Check:** `kubectl describe pod -n dev <pod-name>`
- **Look for:** "Liveness probe failed" or "Readiness probe failed"
- **Fix:** Ensure /health endpoint returns 200

