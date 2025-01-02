## Backup and Restore

- There will be a time when we need to backup and restore the Kubernetes cluster.
- The backup and restore process is important for disaster recovery.

### Backup

- **Backup Candidates**:
    - **ETCD**: The most important data to backup is the `etcd` data. It contains all the cluster data like configurations, secrets, etc.
    - **Resource Configurations**: We can also backup the resource configurations like `Pods`, `Deployments`, `Services`, `ConfigMaps`, `Secrets`, etc.
    - **Persistent Volumes**: We can also backup the persistent volumes. In case we used some in our applications.

#### Backup for Resource Configurations

- The best Practice is to use Declarative approach for creating resources. So that we can store the definition files in a version control system like `git`.
- But it is not necessary to follow. If some resources are created imperatively, we can use the below commands to backup the resources.
```bash
kubectl get all --all-namespaces -o yaml > resources-backup.yaml
```
- There are also some third-party tools like `Velero` to backup the resources.

#### Backup for ETCD

- We can use the `etcdctl` command to backup the `etcd` data.
- The below command will backup the `etcd` data to a file.
```bash
ETCDCTL_API=3 etcdctl --endpoints=https://[127.0.0.0:2379] --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/healthcheck-client.crt --key=/etc/kubernetes/pki/etcd/healthcheck-client.key snapshot save /path/to/snapshot.db
```
- The `etcd` data will be saved to the file `/path/to/snapshot.db`.
- Use the below command to restore the `etcd` data from the backup file.
```bash
etcdctl snapshot restore /path/to/snapshot.db --data-dir /var/lib/etcd --initial-cluster=<initial-cluster> --initial-cluster-token=<token> --initial-advertise-peer-urls=<advertise-peer-urls>
```
- To check the status of the backup, use the below command.
```bash
ETCDCTL_API=3 etcdctl  snapshot status /path/to/snapshot.db
```
- Before restore the snapshot, we need to stop `kube-apiserver` service.
- After successful restore, Start the `kube-apiserver` service again.
- Restart the `etcd` service.


Date of Commit: 09/04/2024

*personal*
1) Where is the ETCD server certificate file located?
inside pod def file of etcd pod 
--cert-file=/etc/kubernetes/pki/etcd/server.crt

2) Where is the ETCD CA Certificate file located?
Check the ETCD pod configuration with the command: kubectl describe pod etcd-controlplane  -n kube-system and look for the value of --trusted-ca-file:
--trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt 
3) key is key_file eg:  --key-file=/etc/kubernetes/pki/etcd/server.key
4) endpoint is in --listen-client-urls=https://127.0.0.1:2379
