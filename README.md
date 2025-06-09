# Kubernetes Security Policies: OPA Gatekeeper + Kyverno

This project enforces Pod Security Standards (PSS) using:
- **OPA Gatekeeper** for validation
- **Kyverno** for conditional auto-mutation (injection)

## Gatekeeper
Role: Rejects Pods missing required `securityContext`, unless labeled with `sec-check=true`.
### Getting Started:
```bash
kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/gatekeeper/release-3.13/deploy/gatekeeper.yaml
```

### Apply Gatekeeper Policies:
```bash
kubectl apply -f gatekeeper/constrainttemplate.yaml
kubectl apply -f gatekeeper/constraint.yaml
```

## Kyverno
Injects secure `securityContext` into Pods *only if* they are labeled with `sec-check=true`.

### Getting Started:
```bash
kubectl apply -f https://raw.githubusercontent.com/kyverno/kyverno/main/config/release/install.yaml
```

### Apply Kyverno Policy:
```bash
kubectl apply -f kyverno/inject-security-context.yaml
```

## 🧪 Test
```bash
kubectl apply -f test/insecure-pod.yaml            
#  Should be rejected by Gatekeeper
kubectl apply -f test/insecure-pod-with-label.yaml 
#  Should be mutated by Kyverno
```
