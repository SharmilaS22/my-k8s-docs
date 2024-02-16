
Service accounts 
- used by machines to access kube api
- create
```bash
kubectl create sa <sa-name>
kubectl create serviceaccounts <sa-name>

k get sa
k describe sa <sa-name>
```

- creates a secret object with token (which is used by service account) to authenticate
- this token - does not have an expiry
---

- In the new version of kubernetes - 1.24
- The token is created separately with expiration time and added to service account to use

---
- by default the pods mounts default service account to use
- cant change the service account for a pod after creating (need to be replaced)
- namespace scoped

To chnage Service accounts for pods
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  serviceAccountName: build-robot
  automountServiceAccountToken: false
```

---
in latest versions (from 1.22), pods has tokens with expiraton date
```
$ k describe pod <pod-name>
...
Volumes:
  kube-api-access-gdqqp:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
```
Also, the token is created separately with expiration and associated to the service account

---
To create old way, secrets with service account - with no expiration, 
create a secret definition file of type 
```yaml
...
kind: Secret
type: kubernetes.io/service-account-token 
metadata:
    annotations:
        kubernetes.io/service-account.name : <sa-name>
...
```

---
