1) kubectl set image deployment/nginx-deploy nginx=nginx:1.17 --record
kubectl annotate deployment nginx-deploy --overwrite message="Updated nginx image to 1.17" //set annotation to the deployemnt
2) how to write commands-
apiVersion: v1
kind: Pod
metadata:
  name: read
  labels:
    run: read
spec:
  containers:
  - name: read
    image: busybox:1.28
    command:
      - sh
      - -c
      - |
        mkdir -p /root/CKA
        nslookup nginx-resolver-service > /root/CKA/nginx.svc
        nslookup nginx-resolver.default.svc.cluster.local > /root/CKA/nginx.pod
        tail -f /dev/null
    volumeMounts:
      - name: config-volume
        mountPath: /root/CKA/
  dnsPolicy: ClusterFirst
  restartPolicy: Always
  volumes:
  - name: config-volume
    emptyDir: {}
3)Task



Solve this question on: ssh cluster1-controlplane


Create an nginx pod called nginx-resolver using the image nginx and expose it internally with a service called nginx-resolver-service. Test that you are able to look up the service and pod names from within the cluster. Use the image: busybox:1.28 for dns lookup. Record results in /root/CKA/nginx.svc and /root/CKA/nginx.pod
Solution

Use the command kubectl run and create a nginx pod and busybox pod. Resolve it, nginx service and its pod name from busybox pod.

To create a pod nginx-resolver and expose it internally:

kubectl run nginx-resolver --image=nginx
kubectl expose pod nginx-resolver --name=nginx-resolver-service --port=80 --target-port=80 --type=ClusterIP

To create a pod test-nslookup. Test that you are able to look up the service and pod names from within the cluster:

kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup nginx-resolver-service
kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup nginx-resolver-service > /root/CKA/nginx.svc

Get the IP of the nginx-resolver pod and replace the dots(.) with hyphon(-) which will be used below.

kubectl get pod nginx-resolver -o wide
kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup <P-O-D-I-P.default.pod> > /root/CKA/nginx.pod

3) whats the difference between --kubeconfig=/etc/kubernetes/kubelet.conf --config=/var/lib/kubelet/config.yaml
ChatGPT said:

Great question! The two flags:

    --kubeconfig=/etc/kubernetes/kubelet.conf

    --config=/var/lib/kubelet/config.yaml

are used by the Kubelet, but they serve different purposes.
🔹 1. --kubeconfig=/etc/kubernetes/kubelet.conf

This flag specifies how the Kubelet communicates with the Kubernetes API server.

    It points to the kubeconfig file that contains:

        API server endpoint (e.g., https://<control-plane>:6443)

        Authentication credentials (certificates, tokens)

        Context and cluster configuration

✅ This is essential for the Kubelet to register with the cluster, receive Pod specs, and report node health.
🔹 2. --config=/var/lib/kubelet/config.yaml

This flag points to the Kubelet’s configuration file, which defines runtime behavior.

    It configures the internal behavior of the Kubelet, including:

        cgroupDriver

        clusterDNS

        authentication/authorization

        eviction thresholds

        staticPodPath

        and many other Kubelet runtime settings

✅ This is used to fine-tune how the Kubelet runs and manages Pods on the node.
✅ Summary
Flag	Purpose
--kubeconfig	Auth info to connect to the API server
--config	Defines how the Kubelet should behave (its settings)
🧠 Think of it like this:

    --kubeconfig is how Kubelet talks to the cluster.

    --config is how Kubelet talks to itself (i.e., runs locally).
