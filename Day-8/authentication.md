## Authentication

- Authentication is the process of verifying the identity of the user.
- It includes authenticating admins accessing the cluster, developer testing the application, end-users accessing the application and other services or application accessing our cluster for some integration purpose.

### Authentication in Kubernetes

- Most of time users like admin and devs need to authenticate with `kube-apiserver` to access the cluster before api server processes the other requests.
- There is many ways to authenticate with kube-apiserver.
- The two most basic ways are:
    - Static Password File - has pass, username, userid (csv-file type)
    - Static Token file - 
    - others are certificates and indentity services(3rd party auth rpotocols like LDAP, Kerberos).
- We need to have our users password or token along with username and user-id in a file.
- Then we have to pass the file to the kube-apiserver by mentioning the file in kube-apiserver.service or kubeapiserver pod def file(if launched as a pod) using the `--basic-auth-file=user-details.csv` or `--token-auth-file=user-details.csv` (if using token file) flags. After updating the .service file, always restart the service. 

- For authenticating a user using basic user and pass, we can authenticate using the `curl -v -k https://master-node-ip:6443/api/v1/pods -u "user1:password123" ` command.

- For static token file, we can authenticate using the `https://master-node-ip:6443/api/v1/pods --header "Authorization: Bearer KpjCVbI7rCFAHYPkBzRb7gu1cUc4B" "Authorization

### Important Note

- This is not a recommended authentication mechanism
- Consider volume mount while providing the auth file in a kubeadm setup
- Setup Role Based Authorization for the new users

### Other notes about kubectl
- We can't create a user using the `kubectl` command.
- But we do have a concept called `ServiceAccount` which is used to authenticate different services to the API server. We can create a `ServiceAccount` using the `kubectl` command.


Date of Commit: 10/04/2024
