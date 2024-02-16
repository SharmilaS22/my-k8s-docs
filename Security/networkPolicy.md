## Network policy

- like a security group for pods
- a resource (shorthand - netpol)
- if attached to a pod, only the requests matching the rules are allowed.

Example

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: my-network-policy
spec:
  podSelector: # on which pod
    matchLabels:
      app: my-app
  policyTypes:
    - Ingress
    - Egress
  ingress:
    - from: # 3 selectors, (or) -> match any of the item in list 
    # (and) selectors within an item in the list 
        - podSelector:
            matchLabels:
              role: backend
        - namespaceSelector: # usecase: restrict ingress within specific env (dev -> dev, qa -> qa)
            matchLabels:
              project: my-project
        - ipBlock:
            cidr: 10.0.0.0/24
            except:
              - 10.0.0.1/32
      ports: # of - <source port>
        - protocol: TCP
          port: 80
  egress:
    - to:
        - podSelector: # AND (match both pod AND namespace labels )
            matchLabels:
              role: database
          namespaceSelector:
            matchLabels:
              project: my-project
        - ipBlock: # OR (match this ip range)
            cidr: 192.168.0.0/16
            except:
              - 192.168.1.0/24
      ports: # to dest port
        - protocol: TCP
          port: 3306
    - ports: # instead of to (can also just provide port and protocol)
        - port: 53
          protocol: UDP
        - port: 53
          protocol: TCP

```

policyTypes: should have ingress or egress or both depending on what rules are needed


On igress, when a request is allowed, the response for the request is allowed back(egress).