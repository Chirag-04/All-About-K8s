
## What is Daemon Set?
In Kubernetes, a DaemonSet is a type of controller that ensures a copy of a specific Pod runs on all (or selected) nodes in a cluster. Unlike regular Deployments, which scale Pods based on replicas, DaemonSets focus on node-level coverage.

Key Points:

* Automatic Pod on Every Node: When a new node is added to the cluster, the DaemonSet automatically schedules a Pod on it.
* Pod Deletion on Node Removal: When a node is removed, the Pods managed by the DaemonSet on that node are also deleted.
* Selective Node Targeting: You can use node selectors, affinities, or tolerations to schedule Pods only on specific nodes.

## Use Cases of DaemonSet:

- Cluster Monitoring: Run monitoring agents (e.g., Prometheus Node Exporter, Datadog agent) on every node.
- Logging: Run log collectors (e.g., Fluentd, Logstash) on all nodes to gather logs.
- Networking: Run network proxies or routers (e.g., Calico, Weave) on all nodes.

## Example 

Code:
```yaml
# Configuration file for Daemon Set
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nginx-ds 
  labels: 
    env: demo
spec:
  template:
    metadata:
      name: nginx-pod
      labels: 
        env: demo
    spec:
      containers:
      - image: nginx
        name: nginx
        ports:
        - containerPort: 80
  selector:
    matchLabels:
      env: demo
```
<img width="961" height="413" alt="Screenshot 2025-09-21 at 6 13 40 PM" src="https://github.com/user-attachments/assets/93e70916-a615-4063-ba7c-a7fa1568f9a4" />

## Does DaemonSetalways deploy on every node?
DaemonSet does not always deploy on every node.

1. Taints & Toleartions: 
   * If a node has a taint (e.g., NoSchedule), Pods won’t be scheduled unless the Pod has the proper toleration.
   * Example: In many clusters, the control-plane node is tainted to prevent scheduling normal workloads. Control-plane nodes often have node-role.kubernetes.io/control-plane:NoSchedule.
3. Node Selectors & Affinity:
   * If you use nodeSelector, nodeAffinity, or podAffinity, then Pods are scheduled only on nodes that match the condition.
   * Example:

```yaml
spec:
  template:
    spec:
      nodeSelector:
        kubernetes.io/hostname: node01
```

👉 DaemonSet Pods will run only on node01, not all nodes.

## Conclusion:

* We can control exactly which nodes a DaemonSet runs on.
* We can make use of labels attached on nodes for pod scheduling
* Example : Let say there are 5 nodes out of them 3 are labelled as prod, 1 as dev and 1 as stage
  So in th daemon set configuration we can right
  ```yaml
  spec:
    template:
      nodeSelector:
         env: prod
  ```










      
