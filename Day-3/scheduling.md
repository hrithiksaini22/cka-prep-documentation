## Scheduling in Kubernetes

### Manual Scheduling

- Kubernetes supports manual scheduling of pods.
- You can schedule a pod to a specific node using nodeName field in the pod definition file.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
    nodeName: node01
    containers:
    - name: myapp-container
        image: nginx
```

- If the nodeName field is not specified, then the pod will be scheduled to a node based on the default scheduling algorithm.
- If the nodeName specified in the pod definition file does not exist, then the pod will remain in the pending state.
- By default this changes will only apply to the next scheduling of the pod. If you want to apply this change to the existing pod, then you need to delete the pod and recreate it. Or you can use the replace command-

k replace --force -f nginx.yaml - it will delete the pod and recreate it with the yaml file

You cant simply moce the pod from one system to another, it can only be destroyed and created in a new node/system.

- To specify a new node for existing pod, We have to create a binding object and then send post request to the pod binding API server.
- Binding object is defined in the below syntax:

```yaml
apiVersion: v1
kind: Binding
metadata:
  name: myapp-pod
target:
  apiVersion: v1
  kind: Node
  name: node01
```
- While sending the request we have to convert the binding object from yaml to json format.

Date of Commit: 05/03/2024

Usually how cube scheduler works is, when a pod definition is applied it looks after the Manifest file and checks whether there is any manual scheduling done or not if there isn't any manual scheduling, then kube scheduler runs the algorithm to filter and find a node to place the pod upon and then After cube scheduler finds a node to deploy the pod upon, it launches a binding object that writes the node name inside the port definition file and then launches the pod inside the node using that binding object
