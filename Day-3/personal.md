get all components in with the labels as env=prod and count how many

k get all --selector env=prod --no-headers | wc -l

controlplane ~ ✖ k get pod --selector bu=finance,tier=frontend,env=prod
NAME          READY   STATUS    RESTARTS   AGE
app-1-zzxdf   1/1     Running   0          9m37s
