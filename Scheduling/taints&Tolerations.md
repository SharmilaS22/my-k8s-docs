## Taints and Tolerations

Taints:
Applied to a Node -> to restrict any type of pod specifically.

Tolerations:
Applied to a Pod to make it _tolerant_ to a par

Eg,
- Node A has a taint app=redis
- Node B does not have a taint.
- Pod A has a toleration app=web
- Pod B has a toleration app=redis
- Pod C has a toleration app=server

Here only Pod B is allowed to be assigned to Node A
Whereas all the pods can be assigned to Node B

The taints can only restrict pods which are not tolerant. It does not mean that all the pods that are tolerant will be assigned to this node. This can be achieved by [Node Affinity](./nodeAffinity.md).

Taint Effects:
1. NoSchedule - (Strictly don't schedule the intolerant pods)
2. PreferNoSchedule - (Doesn't gaurantee no schedule)
3. NoExecute - (Existing pods that are not tolerant will also be evicted and acts like NoSchedule - continue to restrict nontolerant pods)

To add taint to nodes
```
kubectl taint nodes <node-name> <key>=<name>:<taint-effect>

kubectl taint nodes node1 app=redis:NoSchedule
```
 kubectl describe node nodename | grep Taints
 **Taint** case sensitive

To untaint the node (Add a minus symbol at the end)
```
kubectl taint nodes <node-name> <key>=<name>:<taint-effect>-

kubectl taint nodes node1 app=redis:NoSchedule-
```

To add tolerations to pods
```yaml
spec:
    containers:
    tolerations:
    - key: "app"
      operator: "Equal"
      value: "redis"
      effect: "NoSchedule"
```
