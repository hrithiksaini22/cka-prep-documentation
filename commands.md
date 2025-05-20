
## 🧠 **kubectl Imperative Commands Cheatsheet**

### 📦 **Creating Resources**

| Command                                                             | Description                            |
| ------------------------------------------------------------------- | -------------------------------------- |
| `kubectl create deployment <name> --image=<image>`                  | Create a deployment                    |
| `kubectl create service clusterip <name> --tcp=<port>:<targetPort>` | Create ClusterIP service               |
| `kubectl create service nodeport <name> --tcp=<port>:<targetPort>`  | Create NodePort service                |
| `kubectl create namespace <name>`                                   | Create a namespace                     |
| `kubectl create configmap <name> --from-literal=<key>=<value>`      | Create a ConfigMap                     |
| `kubectl create secret generic <name> --from-literal=<key>=<value>` | Create a secret                        |
| `kubectl run <name> --image=<image>`                                | Run a pod (deprecated for deployments) |

---

### 🔁 **Exposing Resources**

| Command                                                        | Description                         |
| -------------------------------------------------------------- | ----------------------------------- |
| `kubectl expose deployment <name> --type=<type> --port=<port>` | Expose a deployment as a service    |
| `kubectl expose pod <name> --port=<port>`                      | Expose a pod directly (for testing) |

---

### ⚙️ **Scaling and Updating**

 Command                                                       | Description                      |
| ------------------------------------------------------------- | -------------------------------- |
| `kubectl scale deployment <name> --replicas=<count>`          | Scale up/down a deployment       |
| `kubectl set image deployment/<name> <container>=<new-image>` | Update the image of a deployment |

---

### 📄 **Viewing Resources**

| Command                              | Description                                       |
| ------------------------------------ | ------------------------------------------------- |
| `kubectl get pods`                   | List all pods                                     |
| `kubectl get all`                    | List all resources in the namespace               |
| `kubectl get svc`                    | List services                                     |
| `kubectl describe <resource> <name>` | Show detailed info about a resource               |
| `kubectl logs <pod-name>`            | Show logs of a pod                                |
| `kubectl top pod`                    | Show pod resource usage (metrics server required) |

---

### 🧹 **Deleting Resources**

| Command                            | Description         |
| ---------------------------------- | ------------------- |
| `kubectl delete pod <name>`        | Delete a pod        |
| `kubectl delete deployment <name>` | Delete a deployment |
| `kubectl delete svc <name>`        | Delete a service    |
| `kubectl delete namespace <name>`  | Delete a namespace  |

---

### 📦 **Config and Troubleshooting**

| Command                                                       | Description                  |
| ------------------------------------------------------------- | ---------------------------- |
| `kubectl config get-contexts`                                 | Show all kubeconfig contexts |
| `kubectl config use-context <name>`                           | Switch context               |
| `kubectl exec -it <pod-name> -- /bin/bash`                    | Open shell inside a pod      |
| `kubectl port-forward pod/<pod-name> <local-port>:<pod-port>` | Port forward to a pod        |


✅ Here's how kubectl commands are categorized:
1. Imperative Commands (What you asked for)

Directly create/update resources, e.g.:

    kubectl create

    kubectl delete

    kubectl apply

    kubectl run

    kubectl expose

    kubectl scale

    kubectl set image

    ⚠️ Note: kubectl apply is often considered declarative, but can be used imperatively with --dry-run or stdin.

2. Informational Commands

To view or inspect resources:

    kubectl get

    kubectl describe

    kubectl logs

    kubectl top

    kubectl events

3. Debugging / Troubleshooting

    kubectl exec

    kubectl port-forward

    kubectl cp (copy files to/from a pod)

    kubectl attach

    kubectl auth can-i

    kubectl debug (for ephemeral containers)

4. Configuration

    kubectl config view

    kubectl config use-context

    kubectl config set-context

    kubectl config set-cluster

    kubectl config set-credentials

5. Advanced Management

    kubectl apply -f (declarative)

    kubectl patch (update partial resources)

    kubectl rollout (status, undo, pause, resume)

    kubectl wait

    kubectl label

    kubectl annotate

    kubectl taint

    kubectl cordon/uncordon/drain (node maintenance)
