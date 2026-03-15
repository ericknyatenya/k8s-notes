# Deployments

> 🚧 Notes in progress

## What is a Deployment?

A Deployment manages a **ReplicaSet**, which in turn manages a set of identical Pods. It gives you:

- **Desired state**: declare how many replicas you want; the controller converges to that
- **Rolling updates**: replace Pods gradually with zero downtime
- **Rollback**: `kubectl rollout undo` to revert to a previous ReplicaSet
- **Self-healing**: if a Pod dies, the ReplicaSet creates a replacement

## When to use

Almost always — if you're running a stateless workload (web server, API, worker), use a Deployment, not a bare Pod.

---

*Examples and annotated YAMLs coming soon.*
