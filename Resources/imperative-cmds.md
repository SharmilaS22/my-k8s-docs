## Imperative commands

to create an nginx pod

```
kubectl run nginx --image=nginx
```

to set cluster config

to view kubectl config

```
kubectl config view
```

in config file, (to set default context - which cluster local/aws/gcp etc)
current-context: <context-name>

---

To get pod.yaml for nginx w/o running it

```
kubectl run nginx --image==nginx --dry-run=client -o yaml
```

To give a container port, use `--port=<port number>`

---

to get deployment.yaml for nginx w/0 creating the deployment

```
kubectl create deployment nginx --image=nginx --dry-client -o yaml
```

to create nginx deployment

```
kubectl create deploy nginx --image=nginx --replicas=2
```

Scaling,

```
kubectl scale deploy nginx --replicas=4
```

---

to get svc.yaml for a pod (the labels of pod are automatically applied as selector for the service)

```
kubectl expose pod redis --port=6379 --name=redis-svc --dry-run=client -o yaml
```

or

```
kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml
```

- This assumes the label for the pod as `app=redis`

To get yaml for svc for NodePort

- can't give nodeport value in cmd (can create file and add it there)
- uses labels of pod as selectors

```
kubectl expose pod nginx --type=NodePort --port=80 --name=nginx-svc --dry-run=client -o yaml
```

or

using 2nd method, can give nodeport but labels are not used as selectors

```
kubectl create service nodeport nginx --tcp:80:80 --nodeport=30001 --dry-run=client -o yaml
```

---

Declarative command
`kubectl apply`
Detects changes and updates the resource / creates if not created already
