## Manual scheduling

The property nodeName could be used to assign a node to a pod manually.
```yaml
spec:
    nodeName: <node-name>
```

Usually when a pod is created, the scheduler finds the best node for it and assigns the pod.
This property nodeName will be updated by the Scheduler. 
We can assign it manually by giving this property

If the pod is not scheduled on a node yet - the status of the pod would be 'Pending'

## Labels and Selectors

Selectors -> to filter pods with certain labels

```sh
kubectl get pods --selector app=redis
or
kubectl get pods -l app=redis
```
To select multiple labels (AND logical)
```sh
kubectl get pods -l app=redis,env=dev
```

**Annotations:**
Key-value pairs but unlike labels, these are not used for identification/grouping. These are non-identifying metadata like build version, release notes etc. They are additonal information not used by k8s but other tools, or for human understanding.
