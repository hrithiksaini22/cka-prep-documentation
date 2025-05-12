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
