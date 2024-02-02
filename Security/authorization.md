
Node authorizer 
- authorizes node (kubelet) - cert with labels 'system:node:*'

ABAC 
- attribute based access control
- set of permissions added to user
- for each change - edit the policy and have to restart the server

RBAC 
- role has permissions which is assigned to users 
- more standard

Web hook 
- 3rd party auth - authorization also is managed by the 3rd 

AlwaysAllow 
- default - allows all access

AlwaysDeny

---
```
authorization-mode=Node,RBAC, Webhook,...
```
the order of the authmode presented here is followed, until any mode authorizes the access