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

*Restore from backup*
First Restore the snapshot:

root@controlplane:~# ETCDCTL_API=3 etcdctl  --data-dir /var/lib/etcd-from-backup \
snapshot restore /opt/snapshot-pre-boot.db


2022-03-25 09:19:27.175043 I | mvcc: restore compact to 2552
2022-03-25 09:19:27.266709 I | etcdserver/membership: added member 8e9e05c52164694d [http://localhost:2380] to cluster cdf818194e3a8c32
root@controlplane:~# 


Note: In this case, we are restoring the snapshot to a different directory but in the same server where we took the backup (the controlplane node) As a result, the only required option for the restore command is the --data-dir.



Next, update the /etc/kubernetes/manifests/etcd.yaml:

We have now restored the etcd snapshot to a new path on the controlplane - /var/lib/etcd-from-backup, so, the only change to be made in the YAML file, is to change the hostPath for the volume called etcd-data from old directory (/var/lib/etcd) to the new directory (/var/lib/etcd-from-backup).

  volumes:
  - hostPath:
      path: /var/lib/etcd-from-backup
      type: DirectoryOrCreate
    name: etcd-data
With this change, /var/lib/etcd on the container points to /var/lib/etcd-from-backup on the controlplane (which is what we want).

When this file is updated, the ETCD pod is automatically re-created as this is a static pod placed under the /etc/kubernetes/manifests directory.



Note 1: As the ETCD pod has changed it will automatically restart, and also kube-controller-manager and kube-scheduler. Wait 1-2 to mins for this pods to restart. You can run the command: watch "crictl ps | grep etcd" to see when the ETCD pod is restarted.

Note 2: If the etcd pod is not getting Ready 1/1, then restart it by kubectl delete pod -n kube-system etcd-controlplane and wait 1 minute.

Note 3: This is the simplest way to make sure that ETCD uses the restored data after the ETCD pod is recreated. You don't have to change anything else.



If you do change --data-dir to /var/lib/etcd-from-backup in the ETCD YAML file, make sure that the volumeMounts for etcd-data is updated as well, with the mountPath pointing to /var/lib/etcd-from-backup (THIS COMPLETE STEP IS OPTIONAL AND NEED NOT BE DONE FOR COMPLETING THE RESTORE)


2) How many clusters are defined in the kubeconfig on the student-node?
      k config get-clusters
3) How many nodes (both controlplane and worker) are part of cluster1?
Make sure to switch the context to cluster1:

kubectl config use-context cluster1
k get nodes

#### notes

**1)** there are two ways in which we can run a service i.e via the pod and other one is starting the app as a daemon service via systemctl.
a) as pod- kubeadm starts the core system services via manifest files saved in /etc/kubernetes/manifests/ dir. Make changes to the pod def file here, eg: updating etcd manifest file to mount new folder, where the new backuup is saved. 

**2)** whwn we start an app as a service , we mention the .service for configuring the app and start it via systemctl start command. 
-the file ca be found in /etc/systemd/system/<service_name>.service path
whenever we edit the service file, we restart the service always to apply the changes. 

**3)** The kubectl config use-context command is used to switch between multiple Kubernetes clusters in your kubeconfig file. When you have multiple clusters (like Cluster1 and Cluster2) configured, each has its own context (combination of cluster, user, and namespace).

By running kubectl config use-context context-name, you tell kubectl which cluster to interact with. Without this command, kubectl won't know which cluster to use when you run commands, so it defaults to the current context.

In summary, it's necessary to use this command to switch between clusters and avoid interacting with the wrong one.

q) How many nodes are part of the ETCD cluster that etcd-server is a part of?
etcd-server ~ ➜  ETCDCTL_API=3 etcdctl \
  --endpoints=https://127.0.0.1:2379 \
  --cacert=/etc/etcd/pki/ca.pem \
  --cert=/etc/etcd/pki/etcd.pem \
  --key=/etc/etcd/pki/etcd-key.pem \
   member list
