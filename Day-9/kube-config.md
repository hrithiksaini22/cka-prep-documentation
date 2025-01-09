## Kube-config

- If we are accessing the Kube-api sever directly, We use the `curl -k https://kube-apiserver-ip:6443` command to access the Kubernetes cluster.
- But we also have to pass the `--cacert`, `--cert` and `--key` flags to access the Kubernetes cluster.
- Example:
```bash
$ curl -k https://kube-apiserver-ip:6443 --cacert ca.crt --cert admin.crt --key admin.key
```
- For using the `kubectl` commands also we need to specify the certificate paths.
- Every time passing the certificate paths is not a good practice.
- Instead of that, We can use the `config` file to store the certificate paths.
- When using the `kubectl` commands, We need to specify the `--kubeconfig` flag to use the `config` file.
- Example:
```bash
$ kubectl get pods --kubeconfig config
```
- But we have also one method to use the `kubectl` commands without specifying the `--kubeconfig` flag.
- We need to store the `config` file in the `$HOME/.kube` directory.
- The `kubectl` command will automatically look for the `config` file in the `$HOME/.kube` directory.

### KubeConfig File:

- The `config` file is a YAML file that contains the details of the Kubernetes cluster.
- The `config` file contains the following details:
    - Cluster details
    - User details
    - Context details
- The `config` file is divided into fivce sections:
    - `apiVersion`
    - `kind`
    - `current-context`
    - `clusters`
    - `users`
- The `apiVersion` and `kind` are the mandatory fields in the `config` file.
- Example:
```yaml
apiVersion: v1
kind: Config
current-context: kubernetes-admin@kubernetes
clusters:
- name: kubernetes
  cluster:
    certificate-authority: ca.crt
    server: https://kube-apiserver-ip:6443
users:
- name: kubernetes-admin
  user:
    client-certificate: admin.crt
    client-key: admin.key
contexts:
- name: kubernetes-admin@kubernetes
  context:
    cluster: kubernetes
    user: kubernetes-admin
    namespace: default
```
- The `config` file contains the details of the `kubernetes` cluster.
- Instead odf specifying the certificate paths every time, We can use directly give the cert content in encoded format. By using `-data` flag after `certificate-authority`, `client-certificate` and `client-key` flags.

### Using the KubeConfig File by commands

- Command to view the details of the `config` file:
```bash
$ kubectl config view
```

- Command to use the set context in the `config` file:
```bash
$ kubectl config use-context kubernetes-admin@kubernetes
```
- Command for config file in another location:
```bash
$ kubectl get pods --kubeconfig /path/to/config
```

Date of Commit: 11/03/2024

***Questions***

1)I would like to use the dev-user to access test-cluster-1. Set the current context to the right one so I can do that.

To use that context, run the command: kubectl config --kubeconfig=/root/my-kube-config use-context research

To know the current context, run the command: kubectl config --kubeconfig=/root/my-kube-config current-context

2) We don't want to specify the kubeconfig file option on each kubectl command.

Set the my-kube-config file as the default kubeconfig file and make it persistent across all sessions without overwriting the existing ~/.kube/config. Ensure any configuration changes persist across reboots and new shell sessions.

A-Add the my-kube-config file to the KUBECONFIG environment variable.

Open your shell configuration file:
vi ~/.bashrc
Add the following line to export the variable:
export KUBECONFIG=/root/my-kube-config
Apply the Changes:

Reload the shell configuration to apply the changes in the current session:

source ~/.bashrc
