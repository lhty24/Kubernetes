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

### Deployments

### Namespaces
Kubernetes supports multiple virtual clusters backed by the same physical cluster. These virtual clusters are called namespaces.
Namespaces are intended for use in environments with many users spread across multiple teams, or projects. 

### Scheduler
In Kubernetes, scheduling refers to making sure that Pods are matched to Nodes so that Kubelet can run them.

A scheduler watches for newly created Pods that have no Node assigned. For every Pod that the scheduler discovers, the scheduler becomes responsible for finding the best Node for that Pod to run on. The scheduler reaches this placement decision taking into account the scheduling principles described below.



### Taint
Node affinity is a property of pods that attracts them to a set of nodes (either as a preference or a hard requirement). Taints are the opposite – they allow a node to repel a set of pods.

Taints and tolerations work together to ensure that pods are not scheduled onto inappropriate nodes. One or more taints are applied to a node; this marks that the node should not accept any pods that do not tolerate the taints. Tolerations are applied to pods, and allow (but do not require) the pods to schedule onto nodes with matching taints.


# Setup Scheduler
- error with step 1 deploy
`kubectl create -f gpushare-schd-extender.yaml`
get error: `error: unable to recognize "gpushare-schd-extender.yaml": no matches for kind "Deployment" in version "extensions/v1beta1"`
refer to issue: https://github.com/kubernetes/minikube/issues/5420

solution: migrate to new api `apps/v1` in the YAML file gpushare-schd-extender.yaml
refer to doc: https://kubernetes.io/blog/2019/07/18/api-deprecations-in-1-16/

### Setup Device Plugin
update api based on https://kubernetes.io/blog/2019/07/18/api-deprecations-in-1-16/
ignore validation error

### Figuring out user guide
- 只运行nvidia-device-plugin-daemonset而不运行gpushare-device-plugin,执行 create -f 1.yaml （创建binpack pod）都没任何结果输出

- 我之前也遇到过一样的问题，我当时是集群的调度器并没有启用gpushare-scheduler-extender，这样pod的annotation中就不会产生该pod应该分配的device ID，在device-plugin执行实际分配时，则会报unknown device id的错误。你可以describe一下你的pod,看annotation中是否有ALIYUN_COM_GPU_MEM_IDX的值.

------COMMANDS--------
- make sure GPU sharing scheduler extender and dev plugin are up
`kubectl get po -n kube-system -l app=gpushare`

- check GPU sharing of the k8s cluster
`kubectl inspect gpushare`

- create deployment binpack-1 which requests 8138MB / 0.5 GPU
`vi 1/1.yaml`
(insert code)

- create deployment binpack-1
`kubectl create -f 1/1.yaml`

- ?
`kubectl get po`

`kubectl inspect gpushare` 

`-d` details: show which pod are placed on which GPU device

