## Resource request and limits

For a container we can provide CPU and memory required for it, so the scheduler will allocate the pod to a node with specified resource requirements available.

```yaml
spec:
    containers:
      - name: webapp
        ...
        resources:
            requests:
                memory: "2Gi"
                cpu: 2
```
cpu - lowest val -> 0.1 or 1m
1 cpu = 1vCPU

memory 256Mi or 256M
1K -> 1000 bytes
1Ki -> 1024 bytes

## Resource usage limits:

to limit using resources from the node.
```yaml
spec:
    containers:
      - name: webapp
        ...
        resources:
            requests:
                memory: "2Gi"
                cpu: 1
            limits:
                memory: "3Gi"
                cpu: 2
```
If a pod exceeds the limit of cpu - the requests throttles 
if exceeds memory limit and exceeds the node memory -> pod would be terminated with OOM Out of Memory 

---
by default - no requests or limits

---
Having set requests and no limits, allow the pod to use up resources when they are available outside it's request.

---
## LimitRange
LimitRange can be set to namespaces
After this is applied, new pods are provided this limitRange of resources.

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-constraint
spec:
  limits:
  - default:
      cpu: 500m
    defaultRequest:
      cpu: 500m
    max: 
      cpu: "1"
    min:
      cpu: 100m
    type: container
```
---
## Resource Quota
namespace level
```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
    name: rsquota
spec:
    hard:
        requests.cpu: 4
        requests.memory: 4Gi
        limits.cpu: 10
        limits.memory: 10Gi
```