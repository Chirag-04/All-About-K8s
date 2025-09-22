## Static pods concept:
* We know that schedular is the component which is response to scheduling pods inside nodes.
* But interestingly schedular is itself a pod and if it's a pod who is response for running it.
* That's where concept of static pods comes up
* Static pods are control plane components that are not managed by schedular
* A Static Pod is a Pod that is managed directly by the kubelet on a specific node, without the API server or controller manager creating it.
* Static Pods are created directly by kubelet on a node.

