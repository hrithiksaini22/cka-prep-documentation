## Static Pods

- Static Pods are managed directly by the kubelet daemon on a specific node, without the API server observing them. Unlike Pods that are managed by the control plane (for example, a Deployment); instead, the kubelet watches each static Pod (and restarts it if it fails).

- Static Pods are always bound to one Kubelet on a specific node.

- The kubelet automatically tries to create a mirror Pod on the Kubernetes API server for each static Pod. This means that the Pods running on a node are visible on the API server, but cannot be controlled from there. The Pod names will be suffixed with the node hostname with a leading hyphen.

### More about Static Pods

- Static Pods are managed directly by the kubelet daemon on a specific node, without the API server observing them. Unlike Pods that are managed by the control plane (for example, a Deployment); instead, the kubelet watches each static Pod (and restarts it if it fails).
- Static Pods are always bound to one Kubelet on a specific node.
- So we have to create the static pod definition file on some specific location on the node and kubelet will take care of the rest.
- We just have to mention the static pod definition file location in the kubelet configuration file.
- There is two option to mention the static pod definition file location in the kubelet configuration
    - `--pod-manifest-path` flag
    - `staticPodPath` field in the kubelet configuration file by using the `--config` flag.
- If the file is created in the location mentioned in the kubelet configuration file, then the kubelet will create the pod from the definition file.
- If the file is deleted from the location mentioned in the kubelet configuration file, then the kubelet will delete the pod.

Date of Commit: 06/03/2024

personal

1) How many static pods exist in this cluster in all namespaces?
   Run the command kubectl get pods --all-namespaces and run k describe pod -> then check the config file and find a section owners reference- there if the "kind" is Node then it is a static pod. If the "Kind" is different then it is n9ot a static pod.
   
   2) What is the path of the directory holding the static pod definition files?
   Run the command ps -aux | grep kubelet and identify the config file - --config=/var/lib/kubelet/config.yaml. Then check in the config file for staticPodPath.
![image](https://github.com/user-attachments/assets/a0fee5ff-daec-45ad-955e-6e103e1a6a4e)

3) how to delete a static pod?
you can simply delete the pod but the kubelet would redeploy the container from the manifest files fiectory it monitors. Hence delete the deployemnt file from the manifest folder and kubelet would recreate the pod after deleting. 
a) check on which node is the pod running- k get pods -A -o wide
b) ssh to the node by - ssh (node01 or use the internal ip of the node)
c) now we have to delete the manifest file so that kubelet doesnt spin up the pods again, hence we fist SSH to node01 and identify the path configured for static pods in this node.

Important: The path need not be /etc/kubernetes/manifests. Make sure to check the path configured in the kubelet configuration file.
now first find the location to the kubelet config file beacuse from there you can find the directory path where the manifest files are present so that kubelet can run the pods using those manifesst files-
every time the path for kubelet config file would be /var/lib/kubelet/config.yaml
and if you want to find the path then just search by- 
ps -aux | grep kubelet
now check for --config path
open the config.yaml
![image](https://github.com/user-attachments/assets/4401e32d-636d-4ffc-9395-9c15a43b7994)
find the static pod path
![image](https://github.com/user-attachments/assets/cf18887b-33f7-4d64-b236-ee3c0cf72f1c)

5) how many static pods are there in the cluster?
   enter command - k get pods -A and then search for the pods with the name of the nodes appended at the end of the pod name-
eg-
![image](https://github.com/user-attachments/assets/2c759deb-f318-4e43-9083-5c9b6f26aee1)
