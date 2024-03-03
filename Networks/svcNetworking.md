Service Networking

- Service is just a k8s object not a server, it's cluster scoped not node specific
- k8s assigns an ip address to the service when it's created.
- The ip range for svc ip is configured in kube api server. Default is 10.0.0.0/24 `--service-cluster-ip-range`
- Kube-proxy in each node manages the port and ip forwarding for the service ip:port to pod ips:port
- The port for the node in  nodeport service is the same for all the nodes.
- kube-proxy uses userspace or ipvs or iptables(default) to manage ip forwarding
- Make sure the pod and svc have dedicated ip cidr ranges with no overlap
- iptables -L -t nat | grep <svc-name>
- /var/log/kube-proxy.log 