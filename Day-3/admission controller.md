What are they?
Admission controllers are code within the Kubernetes API server that check the data arriving in a request to modify a resource.

Admission controllers apply to requests that create, delete, or modify objects. Admission controllers can also block custom verbs, 
such as a request to connect to a pod via an API server proxy. 
Admission controllers do not (and cannot) block requests to read (get, watch or list) objects, because reads bypass the admission control layer.

Admission control mechanisms may be validating, mutating, or both. Mutating controllers may modify the data for the resource being modified; validating controllers may not.

Note that the NamespaceExists and NamespaceAutoProvision admission controllers are deprecated and now replaced by NamespaceLifecycle admission controller.

The NamespaceLifecycle admission controller will make sure that requests
to a non-existent namespace is rejected and that the default namespaces such as
default, kube-system and kube-public cannot be deleted.
