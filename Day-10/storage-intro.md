## Storage Introduction

### Docker container Storage

- Normally the data inside the container is not persistent. If the container is deleted, then the data inside the container will also be deleted.
- To avoid this, We will use persistent storage for the containers.
- In docker to use persistent storage, we can use the `-v` option to mount the volume to the container.
- Or we can also use `--mount` option to mount the volume to the container.

### Volume driver plugins in Docker

- Actually, Docker uses the volume driver plugins to manage the storage drivers. Volume driver plugin provides volumes.
- We can use the following command to list the volume driver plugins:
- local is the deafult volume driver plugin
![WhatsApp Image 2025-01-11 at 21 23 09_4e99386e](https://github.com/user-attachments/assets/5be50a85-8234-486d-9bbc-d60efb6808e6)

```bash
docker volume ls
```
- Depending upon the requirement, we can use the volume driver plugins to manage the volumes.
![WhatsApp Image 2025-01-11 at 21 16 10_91f1386e](https://github.com/user-attachments/assets/ee15c4c1-0c29-46d5-80db-fdd69a5915cd)

### Container Storage Interface (CSI)

- Container Storage Interface (CSI) is a standard for exposing arbitrary block and file storage systems to containerized workloads on Container Orchestration Systems like Kubernetes.
- It is a standard that is implemented by the storage vendors to provide the storage to the containerized workloads.
![WhatsApp Image 2025-01-11 at 21 27 30_71fe355f](https://github.com/user-attachments/assets/5a57dee5-9327-4673-a7e6-d9cf50168b7c)

Date of Commit: 12/03/2024
