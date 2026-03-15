# Pods

## What is a Pod?

A Pod is the **smallest deployable unit in Kubernetes**. It wraps one or more containers that:

- Share the same **network namespace** (same IP, same `localhost`)
- Share the same **storage volumes**
- Are always **co-scheduled** on the same node

> Think of a Pod as a logical host. Containers inside it behave like processes on the same machine.

---

## When to use a Pod directly

**Rarely in production.** Bare Pods have no self-healing — if a node dies or the Pod crashes, it's gone. You typically manage Pods indirectly through:

| Controller | Use case |
|---|---|
| `Deployment` | Stateless apps, rolling updates |
| `StatefulSet` | Stateful apps (DBs, queues) needing stable identity |
| `DaemonSet` | One Pod per node (log agents, node exporters) |
| `Job` / `CronJob` | Run-to-completion workloads |

**When a bare Pod makes sense:**
- Debugging / troubleshooting on a cluster
- One-off tasks during development
- Learning how K8s scheduling and networking work

---

## Key concepts

### Pod lifecycle

```
Pending → Running → Succeeded / Failed / Unknown
```

- **Pending**: Scheduled but containers not yet started (image pull, resource wait)
- **Running**: At least one container is running
- **Succeeded**: All containers exited 0 (common for Jobs)
- **Failed**: At least one container exited non-zero and won't restart

### Restart policies

| Policy | Behavior |
|---|---|
| `Always` | Restart on any exit — default, used for long-running services |
| `OnFailure` | Restart only on non-zero exit — used for Jobs |
| `Never` | Never restart — use when you want exactly one run |

---

## Annotated Examples

### Minimal Pod — imperative equivalent

The manifest below is equivalent to:
```bash
kubectl run nginx-yaml --image=nginx
```

See [`nginx-pod.yaml`](./nginx-pod.yaml) for the declarative version.

### Multi-container Pod (sidecar pattern)

See [`sidecar-pod.yaml`](./sidecar-pod.yaml) — an app container paired with a log-shipping sidecar. Both share a volume.

---

## Commands

```bash
# Create from manifest
kubectl apply -f nginx-pod.yaml

# Generate a manifest without applying (great starting point)
kubectl run nginx-yaml --image=nginx --dry-run=client -o yaml > nginx-pod.yaml

# List pods (current namespace)
kubectl get pods

# Full detail + events (first place to look when something is broken)
kubectl describe pod nginx-yaml

# Stream logs
kubectl logs -f nginx-yaml

# Exec into a running pod
kubectl exec -it nginx-yaml -- /bin/bash

# Delete pod
kubectl delete pod nginx-yaml
# or
kubectl delete -f nginx-pod.yaml
```

---

## Gotchas

- `creationTimestamp: null` and `status: {}` appear when you use `--dry-run=client -o yaml`. They're placeholders — fine to leave in, fine to strip before committing.
- `resources: {}` means no requests/limits are set. Fine for dev, **never do this in prod** — a Pod with no limits can starve other workloads on the node.
- A Pod that's `Running` but not `Ready` means its readiness probe is failing. The Service will not route traffic to it.
