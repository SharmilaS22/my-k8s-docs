## Namespaces:

### Automatically created:
- default
- kube-system
    - has pods, svc, etc needed by k8s 
- kube-public
    - resources that are made available to all the users
---
- For maintaining different environments like dev, test, prod with specific resource policies.
- Prevents accidental deletion of resources in other/higher environments
---
- A resource can access another resource in the same namespace directly with it's name.
- A resource can access another resource from a different namespace need the namespace name along with the resource's name.

Eg, 

app(pod from ns A) --> db-service(service from ns A)
```
mysql.connect("db-service")
```

app(pod from ns A) --> db-service(service from ns B)
```
mysql.connect("db-service.B.svc.cluster.local")
```
- cluster.local -> domain
- svc -> service subdomain
- B -> namespace name
- db-service -> svc name

---
To use in commands:
```
kubectl apply -f pods.yaml --namespace <namespace-name>
```
OR
- in pods.yaml:
```
metadata:
    namespace: <namespace-name>
```
---

To set a default *value* for namespace:
```
kubectl config set-context $(kubectl config current-context) --namespace <namespace-dev>
```
---
To get any resources in all namespaces:
```
kubectl get pods --all-namespaces
```
To assign a resource quota for a namespace:
```
apiVersion: v1
kind: ResourceQuota
metadata:
    namespace: <namespace-name>
    name: sample-resource-quota
spec:
    hard:
        pods: "10"
        requests.cpu: "4"
        requests.memory: "5Gi"
        limits.cpu: "10"
        limits.memory: "10Gi"
``` 
