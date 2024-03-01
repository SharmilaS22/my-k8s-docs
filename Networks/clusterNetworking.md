## Ports - 
[doc](https://kubernetes.io/docs/reference/networking/ports-and-protocols/)

### Master node: (inbound)
- api server - 6443 
- kubelet - 10250
- scheduler - 10259
- controller - 10257
- etcd - 2379(for all control plane components), 2380(for etcd-to-etcd connections - in case of multiple master nodes)
### Worker node: (inbound)
- Node ports - 30000 to 32767
- kubelet - 10250

