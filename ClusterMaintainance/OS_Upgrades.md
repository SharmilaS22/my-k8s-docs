## OS Upgrades

When a node goes down,  the pods in the node will be unaccessible. The kube scheduler will wait for a period of time. If the pod is not back online after the timeout, the scheduler will create the pod (replica set) in another node.

The timeout is called pod eviction timeout, which can be set in kube controller manager.

The default timeout is 5 mins.

```
kubectl drain node01
```
The pods in node _node01_ is terminated and recreated in other nodes. And no pods are assigned to this node until, we run the following command.
```
kubectl uncordon node01
```
To just make sure no new pods are scheduled on this node, note: this does not terminate any pod and recreate it in other nodes
```
kubectl cordon node01
```
> Note: Only new pods that are created after this will be scheduled on this node. The old pods which were running before drained will not be rescheduled back to this node.

Nodes that are not managed by ReplicaSets/deployments will just be lost and not recreated.

---

Upgrading node one by one, 
refer upgrading kubeadm in k8s.io

```
kubectl drain node node01
<--in the node-->
apt-get upgrade -y kubeadm=<version>
apt-get upgrade -y kubelet=<version>
kubeadm upgrade node config --kubelet-version <version>
systemctl restart kubelet
<--->
kubectl uncordon node01
```

```
kubeadm upgrade plan
```