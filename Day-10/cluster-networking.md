## Cluster Networking

- In cluster Each node will have its own IPv4 address, own hostname and its own IPV6 address.
- The master nodes consists of various components like API server, Controller manager, Scheduler, etcd and kubelet.
- And worker nodes will have kubelet, kube-proxy.
- So Each service listens on a specific port inside the cluster.
- We need to open the ports in the firewall to allow the traffic to the services. So that all components can communicate with each other.

Refer Kuberenetes official documentation for ports information: [K8S Ports](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#check-required-ports)

Date of Commit: 12/03/2024

1) to find which interface is the bridge inetrface-
controlplane ~ ✖ ip addr show type bridge
5: cni0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1360 qdisc noqueue state UP group default qlen 1000
    link/ether c2:b9:a8:8c:96:66 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/24 brd 172.17.0.255 scope global cni0
       valid_lft forever preferred_lft forever
    inet6 fe80::c0b9:a8ff:fe8c:9666/64 scope link 
       valid_lft forever preferred_lft forever
2) to find the port on which the service is running-
controlplane ~ ✖ netstat -npl | grep -i kube
tcp        0      0 127.0.0.1:10249         0.0.0.0:*               LISTEN      4715/kube-proxy     
tcp        0      0 127.0.0.1:10248         0.0.0.0:*               LISTEN      4152/kubelet        
tcp        0      0 127.0.0.1:10257         0.0.0.0:*               LISTEN      3876/kube-controlle 
tcp        0      0 127.0.0.1:10259         0.0.0.0:*               LISTEN      3361/kube-scheduler 
tcp6       0      0 :::6443                 :::*                    LISTEN      3378/kube-apiserver 
tcp6       0      0 :::8888                 :::*                    LISTEN      4286/kubectl        
tcp6       0      0 :::10256                :::*                    LISTEN      4715/kube-proxy     
tcp6       0      0 :::10250                :::*                    LISTEN      4152/kubelet        
unix  2      [ ACC ]     STREAM     LISTENING     3752449067 4152/kubelet         /var/lib/kubelet/device-plugins/kubelet.sock
unix  2      [ ACC ]     STREAM     LISTENING     3752442508 4152/kubelet         /var/lib/kubelet/pod-resources/4015613891

3) Notice that ETCD is listening on two ports. Which of these have more client connections established?
controlplane ~ ➜  netstat -npl | grep -i etcd
tcp        0      0 127.0.0.1:2379          0.0.0.0:*               LISTEN      3665/etcd           
tcp        0      0 127.0.0.1:2381          0.0.0.0:*               LISTEN      3665/etcd           
tcp        0      0 192.168.163.11:2379     0.0.0.0:*               LISTEN      3665/etcd           
tcp        0      0 192.168.163.11:2380     0.0.0.0:*               LISTEN      3665/etcd           
    
controlplane ~ ➜  netstat -npa | grep -i etcd | grep -i 2379 | wc -l
61

controlplane ~ ➜  netstat -npa | grep -i etcd | grep -i 2380 | wc -l
1
