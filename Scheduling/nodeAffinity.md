
## Node Selectors
in the pod.yaml,
```
Spec:
    ...
    NodeSelector:
        size: Large
```

Label your nodes using,
```
kubectl label node <node-name> <key>=<value>

kubectl label node node1 size=Large
```

Limitations:
- Logical OR and NOT cannot be accomplished when selecting the labels

## Node Affinity
Complex logic can be implemented using node affinity

in pod.yaml
```
spec:
    affinity:
        nodeAffinity:
            requiredDuringSchedulingIgnoredDuringExecution:
                nodeSelectorTerms:
                  - matchExpressions:
                      - key: size
                        operator: In
                        values:
                         - Large
                         - Small
```
For not operator
```
  - matchExpressions:
    - key: size
    operator: NotIn
    values:
    - small
```
For a key exists;
```
  - matchExpressions:
    - key: size
    operator: Exists
```
### Node Affinity types:
- requiredDuringSchedulingIgnoredDuringExecution
    - like NoSchedule
- preferredDuringSchedulingIgnoredDuringExecution
    - like perferNoSchedule, if node available - schedule
- requiredDuringSchedulingRequiredDuringExecution
    - like noExecution - no scheduing or already executing pods

During execution, if the node label changes, based on the affinity types the pods are ignored or stopped.

---
For a dedicated node for specific type of pods,
Use both taints&tolerations and Node Affinity

Taint the nodes & tolerate the specific pods -> so other type of pods are restricted from node

Give affinity to the pods to match the specific node -> so the pods will always find its appropriate node

---