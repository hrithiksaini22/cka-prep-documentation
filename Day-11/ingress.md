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
