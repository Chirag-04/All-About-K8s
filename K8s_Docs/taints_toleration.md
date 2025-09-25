# Tains and Toleration
A taint is like putting a "restricted entry" sign on a node.
It tells Kubernetes: “Don’t schedule pods here unless they have special permission.”

A taint has three parts:
* Key – identifier (e.g., key=value)
* Effect – what happens if a pod doesn’t tolerate it
  * NoSchedule → pods won’t be scheduled { No new pods will be schedule on this node if a pod doesn't tolerate the taint }
  * PreferNoSchedule → avoid scheduling, but not strict
  * NoExecute → evict running pods + block new ones { No new pods + eviction of already existing pods who doesn't tolerate the taint}
* Value – additional info (optional)


```bash
kubectl taint nodes node1 dedicated=database:NoSchedule
```
This command applies a taint on node1 with the key–value pair dedicated=database and the effect NoSchedule.Existing Pods that don’t have the matching toleration will continue running on this node.New Pods that don’t tolerate this taint will not be scheduled on node1.






## Tolerations
A toleration is like giving a pod a “special entry pass”.It allows the pod to be scheduled on a node with a taint, but it doesn’t force it there.

Example pod toleration:
```yaml
tolerations:
- key: "dedicated"
  operator: "Equal"
  value: "database"
  effect: "NoSchedule"
```
This pod can now run on nodes tainted with dedicated=database:NoSchedule.

Here’s the subtle point:

* The taint on the node defines → key=value:effect.
* The toleration on the pod must match → key, operator, value, and effect.
* Effect is truly defined at the node level → it’s part of the taint (NoSchedule, PreferNoSchedule, or NoExecute).In the pod’s toleration, you just repeat that effect so the scheduler knows which taint rules this pod can tolerate.





## Use Cases

* Dedicated Nodes → e.g., only DB pods run on “database” nodes.
* Isolate GPU Nodes → only ML pods with tolerations can run on GPU nodes.
* Maintenance Mode → taint a node maintenance=true:NoExecute to evict pods during upgrades.
* Critical System Pods → kube-proxy, DNS, etc., tolerate taints so they run everywher





## Workflow

* Admin adds a taint on a node→ Node becomes restricted.
* Scheduler checks if a pod has a matching toleration.
  * If yes → pod is allowed.
  * If no → pod is rejected from that node.
* If NoExecute taint → already running pods without toleration are evicted.


