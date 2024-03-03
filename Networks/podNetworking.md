Pod Networking:

Network model
- every pod should have an IP address
- every pod should be able to communicate with every other pod in the same node
- every pod should be able to communicate with every other pod in another node

The kubelet will have information on which CNI plugin to use.
The cni bin and cni config are added to the kubelet service when it starts



Weave
```sh
kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml
```

To get info on kubelet, 
`ps -aux | grep kubelet`

cni in `/opt/cni/bin`

cni config options in `/etc/cni/net.d/file.conflist`

## IP Address Management (IPAM)
- host-local plugin of cni - helps in managing the free and used ips by the pods in the nodes.
- weave - assigns 10.32.0.0/12(default) for the network (around 1.05 million ips)

`ip addr show <eth-name>` for viewing the ip ranges
