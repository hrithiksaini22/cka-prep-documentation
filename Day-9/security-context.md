## Security Context
by default docker runs the containers as root user hence it shares all the resources but  docker also restricts the containerso that it cant reboot or perform the operations that can disrupt the host. You can still add capabilities using the --cap-add= 0r --cap-drop= while running the container.

![WhatsApp Image 2025-01-10 at 02 21 28_b4e61d2c](https://github.com/user-attachments/assets/d8028887-0410-48c8-9c67-5b2d56771d2f)


- Security Context is a property of the pod or container that defines the security settings for a pod or container.
- For Example, We can define the user who can run the process inside the container, we can define the group, we can define the capabilities, we can define the SELinux options, we can define the AppArmor profile, we can define the seccomp profile, etc.
- We can define the security context at the pod level and at the container level.
- If we define the security context at the pod level, then all the containers in the pod will have the same security context.
- But if we define the security context at the container level, then the container will have its own security context and can override the pod level security context.
- We can define the security context in the pod definition file as follows:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: security-context-demo
spec:
  securityContext:
    runAsUser: 1000
  containers:
  - name: security-context-demo
    image: nginx
    securityContext:
      capabilities:       # Capabilities are used to give additional permissions to the process. It can be only defined at the conatiner Level
        add: ["NET_ADMIN"]
```

Date of Commit: 11/03/2024
