## Control Plane Components

### API Server (kube-apiserver)
This is the central communication hub of Kubernetes. Every request — whether it’s from kubectl, dashboard, or another service — first reaches the API server. It validates the request, processes it, and then either updates the cluster state in etcd or passes instructions to other control plane components.

### etcd
This is the database of Kubernetes. It stores the entire cluster state in a key-value format. Anything you create — a pod, service, or config — is recorded here. When the cluster restarts, Kubernetes rebuilds everything by reading data from etcd.

### Controller Manager (kube-controller-manager)
This component runs different controllers that continuously watch the cluster state. If the actual state does not match the desired state, the controller makes changes to fix it. For example, if a pod crashes, the replication controller creates a new one.

### Scheduler (kube-scheduler)
This is the decision maker for pod placement. When a new pod needs to be created, the scheduler checks all available worker nodes, evaluates resources, and chooses the best node where the pod should run. It only assigns; the kubelet actually executes the assignment.

## Worker Plane Components

### Kubelet
This runs on every worker node and acts as the agent. It receives instructions from the API server (like “run this pod here”) and ensures the containers are actually running on the node. If something goes wrong with a container, the kubelet tries to restart it.

### Kube-Proxy
This is responsible for networking within the cluster. It manages routing rules so that services can talk to each other across nodes. It also provides load balancing for distributing traffic to multiple pods behind a service.

### Container Runtime
This is the actual engine that runs containers. Kubernetes itself does not run containers; instead, it asks a runtime like Docker, containerd, or CRI-O to do the work. The runtime pulls container images, starts them, and manages their lifecycle.