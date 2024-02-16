
Authentication
Authorization
Network policies


Authentication:
- Admins
- Developers
- End Users - app security
- Bots


User -> admin, developer
Service Account -> bots

Auth mechanisms:
1 static password file
2 static token file
3 certificates
4 Identity services 3rd party


- Static Password file - basic, not recommended <Deprecated>
    -> .csv with rows of password, username, userId, groupId(optional)
    -> basic-auth-file
    -> `curl ... -u username:password`
- Static Token file    - basic, not recommended <Deprecated>
    -> .csv with rows of token, username, userId, groupId(optional)
    -> token-auth-file
    -> `curl ... --header "Authorization: Bearer <token>"`

## Role based access control - RBAC

Role and RoleBindings are namespace based

Role
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata: 
    name: developer
rules:
  - apiGroups: [""] //"" for core
    resources: ["pods"]
    verbs: ["list", "get", "create", "update"]
  - apiGroups: [""] //"" for core
    resources: ["services"]
    verbs: ["list", "get", "create"]
    resourceNames: ["node-service", "my-svc"] //only the svc's with these name are allowed to be accessed
```

Role Binding

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
    name: my-role-binding
    namespace: my-namespace //if not specified - default
subjects:
  - kind: User
    name: my-user
    apiGroup: rbac.authorization.k8s.io
roleRef:
    kind: Role
    name: developer
    apiGroup: rbac.authorization.k8s.io
```

kubectl get roles
kubectl get rolebindings
kubectl describe role <role>
kubectl describe rolebinding <rolebinding>


To check whether you have permission on specific verb and resource

```sh
kubectl auth can-i <verb> <resource>

kubectl auth can-i create deployments
> yes

//to check for a specific user
kubectl auth can-i <verb> <resource> --as <user> --namespace <namespace>
kubectl auth can-i delete pods --as my-user
> no

kubectl auth can-i <verb> <resource> --as <user> --namespace <namespace>
```

## Namespace scoped:
- pods, rs, jobs, deployments, svc, secrets, roles, rolebindings, configmaps, pvc
## Cluster scoped:
- nodes, persistent volns, clusterroles, cluster role bindings, certificate signing requests, namespaces

To view more , use filter --namespaced=true/false in `kubectl api-resources --namespaced=true` or `kubectl api-resources --namespaced=false`


To create a non namespace role and role binding (not restricted to a namespace):
use cluster role and cluter role binding, with same syntax.

