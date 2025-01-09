## Certificates API

- Let's say there a new user in the organization and we want to give the user access to the Kubernetes cluster.
- The new user will generate the certificate signing request and send it to the cluster admin.
- The cluster admin will sign the certificate using the CA certificate and send it back to the user.
- The user can use the signed certificate to access the Kubernetes cluster.

### Certificates API:

- We can automate the above process using the Certificates API.
- The Certificates API is a Kubernetes API that allows us to manage the certificates in the Kubernetes cluster.
- We have to create a CertificateSigningRequest object in the Kubernetes cluster.
- CertifcateSigningRequest object contains the certificate signing request. Use the below file to create the CertificateSigningRequest definition file.
    
```yaml
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: user-request
spec:
    request: <base64 encoded CSR>
    signerName: kubernetes.io/kube-apiserver-client
    usages:
    - digital signature
    - key encipherment
    - client auth
```
- After creating the CertificateSigningRequest object, the cluster admin will approve the request.
- The cluster admin can approve the request using the below command.
    
```bash
$ kubectl certificate approve user-request
```
- We can also view pending requests using the below command.
    
```bash
$ kubectl get csr
```

- We can view the details of the certificate using the below command.
    
```bash
$ kubectl get csr user-request -o yaml
```

### Component used in the Certificates API:

- The Controller Manager component in the Kubernetes cluster is responsible for managing the CertificateSigningRequest objects.
- The Controller Manager component will approve the request and sign the certificate using the CA certificate.
- We just have to create the CertificateSigningRequest object and the Controller Manager will take care of the rest.
- We need to mention the `ca.crt and ca.key` file in the `--cluster-signing-cert-file` and `--cluster-signing-key-file` flags in the Controller Manager component.

![WhatsApp Image 2025-01-08 at 22 50 10_b1a6b1e7](https://github.com/user-attachments/assets/b68730c2-f26f-4e1f-a525-5aac60ac6ce2)

Date of Commit: 11/03/2024
