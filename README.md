# ☸️ Kubernetes Notes

Personal K8s reference — concepts, manifests, and annotated examples compiled during homelab work and SRE study.

> Each subdirectory covers a core K8s concept: what it is, when to use it, key fields explained, and working YAML examples.

---

## 📂 Structure

| Directory | Concept |
|---|---|
| [`pods/`](./pods/) | The atomic unit — running containers in K8s |
| [`deployments/`](./deployments/) | Declarative rollouts, scaling, and self-healing |
| [`services/`](./services/) | Stable networking / service discovery |
| [`configmaps-secrets/`](./configmaps-secrets/) | Externalizing config and sensitive data |
| [`namespaces/`](./namespaces/) | Logical cluster isolation |
| [`rbac/`](./rbac/) | Role-based access control |
| [`storage/`](./storage/) | PersistentVolumes, PVCs, StorageClasses |
| [`networking/`](./networking/) | Ingress, NetworkPolicies |
| [`probes/`](./probes/) | Liveness, readiness, startup probes |
| [`resources-limits/`](./resources-limits/) | CPU/memory requests and limits |

---

## 🧠 Conventions

- Every topic directory has a `README.md` with concept notes
- YAML examples are annotated inline with `#` comments explaining *why*, not just *what*
- `commands.md` in each dir lists the most useful `kubectl` commands for that concept

---

## 🛠 Environment

- Cluster: Rancher Desktop (local, `nyate`)
- `kubectl` context: `rancher-desktop`
- K8s version: check with `kubectl version --short`

---

## 📌 Quick Reference

```bash
# Set context
kubectl config use-context rancher-desktop

# Check what's running
kubectl get all -A

# Dry-run a manifest (validate without applying)
kubectl apply -f <file>.yaml --dry-run=client

# Describe a resource for event/status detail
kubectl describe pod <pod-name>

# Stream logs
kubectl logs -f <pod-name>

# Get into a running container
kubectl exec -it <pod-name> -- /bin/sh
```
