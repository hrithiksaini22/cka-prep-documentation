## ConfigMaps

- Configmaps allows us to store configuration data in key-value pairs. We can store non-sensitive data in ConfigMaps.
- Pods can consume ConfigMaps as environment variables, command-line arguments, or as configuration files in a volume.

### Create a ConfigMap

- We can create a ConfigMap using the `kubectl create configmap` command.
- Use the below command to create a ConfigMap from a literal value:   
```shell
kubectl create configmap <configmap-name> --from-literal=<key>=<value>
```
- Use the below command to create a ConfigMap from a file:
```shell
kubectl create configmap <configmap-name> --from-file=<path-to-file>
```
- Using Definition file:
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: <configmap-name>
data:
  <key>: <value>
```

### View a ConfigMap

- Use the below command to view a ConfigMap:
```shell
kubectl get configmaps
```

### View the details of a ConfigMap

- Use the below command to view the details of a ConfigMap:
```shell
kubectl describe configmap <configmap-name>
```

### Use a ConfigMap as environment variables

- Use the below definition file to use a ConfigMap as environment variables using `envFrom` field which inserts alls the values fron the configmap config file:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: <pod-name>
spec:
  containers:
  - name: <container-name>
    image: <image-name>
    envFrom:
    - configMapRef:
        name: <configmap-name>
```

- Use the below definition file to use a ConfigMap as environment variables using `env` field to insert selected values from the config map:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: <pod-name>
spec:
  containers:
  - name: <container-name>
    image: <image-name>
    env:
    - name: <key>
      valueFrom:
        configMapKeyRef:
          name: <configmap-name>
          key: <key>
```

### Use a ConfigMap as a file in a volume

- Use the below definition file to use a ConfigMap as a file in a volume:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: <pod-name>
spec:
  containers:
  - name: <container-name>
    image: <image-name>
    volumeMounts:
    - name: <volume-name>
      mountPath: <path-to-mount>
    volumes:
    - name: <volume-name>
      configMap:
      name: <configmap-name>
```

Date of Commit: 08/03/2024

``` bash
---
apiVersion: v1
kind: Pod
metadata:
  labels:
    name: webapp-color
  name: webapp-color
  namespace: default
spec:
  containers:
  - env:
    - name: APP_COLOR
      valueFrom:
       configMapKeyRef:
         name: webapp-config-map
         key: APP_COLOR
    image: kodekloud/webapp-color
    name: webapp-color
```
