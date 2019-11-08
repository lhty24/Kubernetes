# Kubernetes

### Container Deployment
picture [https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/]

### What it does

- Service discovery and load balancing

- Storage orchestration

- Automated rollouts and rollbacks

- Automatic bin packing

- Self-healing

- Secret and configuration management

### Taint
Node affinity is a property of pods that attracts them to a set of nodes (either as a preference or a hard requirement). Taints are the opposite – they allow a node to repel a set of pods.

Taints and tolerations work together to ensure that pods are not scheduled onto inappropriate nodes. One or more taints are applied to a node; this marks that the node should not accept any pods that do not tolerate the taints. Tolerations are applied to pods, and allow (but do not require) the pods to schedule onto nodes with matching taints.
