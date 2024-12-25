## Node Selectors

Node selectors are used to specify a node that a pod should run on. This is useful when you have a specific node that has a specific hardware or software configuration that is required by the pod.

### Node Selector Definition

- Node selectors are defined in the pod definition file.
- Use the below syntax to define a node selector in a pod definition file:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
    containers:
    - name: myapp-container
        image: nginx
    nodeSelector:
        size: Large
```

But before this we should have a label assigned to our node so that nodeselector can fetch the key/value to assgin the pod to the node acc to the nodeSelector.

To assign a label to your node you can run this command-

k label node <nodename> key=value
k label node node-1 size=Large

- The above node selector will ensure that the pod is scheduled on a node with the label size=Large.

Date of Commit: 05/03/2024
