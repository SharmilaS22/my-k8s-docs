
DNS name for service:

By default, for any service created, the name of the svc and it's corresponding IP address is added to the DNS managed by K8s.
- from within the same namespace the svc name is enough to reach the svc.
- But when accessing a svc from a different namespace, use FQDN
- svc-name.namespace-name.type.root-domain
- `db-service.default.svc.cluster.local`
- cluster.local is default root domain in most cases.


DNS for pod can be explicitly set, if set - the pod name would be assigned by k8s itself - IP address '.' replaced with '-'.
- eg 10.180.10.1 - 10-180-10-1


### Core DNS
- a Deployment of replica set - 2 pods (redundancy)
- The nameserver is the k8s dns server - can add pods ip and name('-' instead of '.') in DNS server. The pods are configured with the nameserver (k8s dns) in /etc/resolv.conf
- all dns configs like health check, cache, relooad, top level domain name etc are set in `/etc/coredns/Corefile`
- This corefile is added to pod as a config map - which can be edited
- The Coredns also have a service (kube-dns svc) which is the nameserver ip
- `host <svc-name>` command to view ip of the svc - which shows FQDN for the svc (even without specifying the full name, because the search 'domains' are also added to resolv.conf)
- 