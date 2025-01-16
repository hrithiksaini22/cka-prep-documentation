***Ingress***
An ingress is used to expose our applications within a cluster using a single endpoint that could use routes/paths to redirect the traffic to the respective apps. 
Ingress helps us provide load balancing and acts as layer 7 load balancer within the cluster.
Ingresss is deployeed using Ingress controller and Ingress resources. The controller deploys the ingress and resource is used to config the ingress.

1) Ingress controller could be deployed using the GCE, Nginx, contour, HAProxy, istio.
we use nginx as the ingress controller.
The nginx ingress controller is deployed just like a deployment file- having separate nginx image. The nginx image also has some configuration parameters to be set 
like err-log-path, ssl-protocols, keep alive probs, session timeouts etc. For this we use configMap and attach that configMap into the manifest file using --configMap=
in the args field, of the nginx ingress controller, you also kick start the nginx service inside the container hence we use args- nginx-ingress-controller to start
it, we also mention the pods on which the controller would be attached , and the container port of the nginx controller. We now habe to expose our controller using node 
port service so that we can proxy the traffic from load balancer to the ingress controller. the ingress controller is also intelligent to minitor the k8 resources and
make essential changes to the cluster, hence it requires a service account with certificates to do that, hence we create thaat.

to setup an ingress controller, we need to hace these components setup.
![WhatsApp Image 2025-01-15 at 11 15 07_8c7a7d78](https://github.com/user-attachments/assets/2f6e5486-f699-4aaf-9aae-2ae4dc48d740)

2) Now once we have setup our nginx ingress controller, we mention the rules to navigate to our apps using the ingress resource file.
![WhatsApp Image 2025-01-15 at 11 56 15_7747ab6b](https://github.com/user-attachments/assets/dc57a064-40ea-4b94-bc1c-8defae54172e)

This entire explanation covers how to use the **Ingress** resource in Kubernetes, specifically with the **NGINX Ingress controller**, to configure URL rewrites when routing traffic to backend services. Let me break down the key points and concepts mentioned in the explanation:

### **Context:**
You are working with two web applications: a **watch app** and a **wear app**, which are both hosted as services in your Kubernetes cluster. These applications display different webpages, and their URLs look like this:

- **Watch App:** `http://<watch-service>:<port>/`
- **Wear App:** `http://<wear-service>:<port>/`

You want to set up an **Ingress** rule so that when users visit certain paths on a public URL (such as `/watch` and `/wear`), the traffic gets forwarded internally to the respective services without the `/watch` and `/wear` parts in the URL.

### **Problem without the Rewrite Target:**
Without the rewrite, if you set up an Ingress like:

```yaml
http://<ingress-service>:<ingress-port>/watch --> http://<watch-service>:<port>/watch
```

The `/watch` part would be appended to the URL of the backend service, but your backend application (the **watch app**) is not configured to handle the `/watch` path. It only knows how to serve content at the root (`/`). As a result, this would lead to a **404 Not Found** error because the app doesn't know about the `/watch` path.

### **The Solution - Using `rewrite-target`:**
To fix this, you can use the **rewrite-target** option. The idea is to modify (rewrite) the incoming URL path before it is forwarded to the backend service.

#### What Does `rewrite-target` Do?
The `nginx.ingress.kubernetes.io/rewrite-target` annotation allows you to rewrite the URL path as it is passed to the backend service. In your case, you want to remove the `/watch` or `/wear` prefix when forwarding the request to the backend application.

For example, the Ingress rule might look like this:

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: test-ingress
  namespace: critical-space
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /watch
        backend:
          serviceName: watch-service
          servicePort: 8080
      - path: /wear
        backend:
          serviceName: wear-service
          servicePort: 8081
```

Here, the `nginx.ingress.kubernetes.io/rewrite-target: /` annotation tells the Ingress controller to strip off the `/watch` and `/wear` parts from the URL and forward the request to the backend services at the root path (`/`).

#### What Happens with this Configuration?
- When a user visits: `http://<ingress-service>:<ingress-port>/watch`
  - The request will be rewritten to: `http://<watch-service>:<port>/`
- Similarly, when a user visits: `http://<ingress-service>:<ingress-port>/wear`
  - The request will be rewritten to: `http://<wear-service>:<port>/`

In this way, you avoid the `/watch` or `/wear` being passed to the backend services, which would otherwise lead to a 404 error since the backend apps are not aware of those paths.

### **Advanced Rewrite Example:**
You can also use regular expressions for more complex rewrites. Here's an example where the path `/something/abc` can be rewritten so that the `/something/` part is stripped out:

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /$2
  name: rewrite
  namespace: default
spec:
  rules:
  - host: rewrite.bar.com
    http:
      paths:
      - path: /something(/|$)(.*)
        backend:
          serviceName: http-svc
          servicePort: 80
```

In this case:
- The path `/something/abc` is matched by the regex `(/|$)(.*)`, and it is rewritten to `/abc` before being forwarded to the backend service.
- The `$2` part refers to the second captured group in the regular expression, which is the part of the URL after `/something/`.

### **Summary:**
The main goal of this configuration is to **rewrite incoming URL paths** before passing the request to the backend services. This ensures that your backend applications receive the correct URL structure (usually without any path prefix like `/watch` or `/wear`) and can respond without errors.

- **Rewrite Target** helps you control and customize the URL path that's forwarded to the backend.
- It simplifies routing when backend services expect requests at the root path (`/`) and not with a specific prefix like `/watch` or `/wear`.
