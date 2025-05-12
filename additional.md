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
