
To provide access to k8s for private images: <[ref](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/#create-a-secret-by-providing-credentials-on-the-command-line)>

Create a secret of type docker-registry
```
kubectl create secret docker-registry regcred --docker-server=<your-registry-server> --docker-username=<your-name> --docker-password=<your-pword> --docker-email=<your-email>
```
add the secret to 
pod definition 
```yaml
spec:
    containers:
    ...
    imagePullSecrets:
    - name : <name of the reg cred secret>
```


---

## Security Context

Security in Docker images
- the docker command is run using the root user, but can run the commands with some other user as well, by specifying
   - in dockerfile `USER sampleuser`
   - in command line, while running, `docker run --user=sampleuser ...`

But the root user privileges of the docker cmd is limited - for security reasons. The docker command can only have few linux capabilities (except the capabilities which might be dangerous to provide)

Though other capabilities can be provided to docker while running if needed. using `--cap-add` option.



# Security in k8s

The user info can be provided in pod level or container level
- The container privileges overrides the pod's privileges
(Capabilities can be provided in container level)
```yaml
...
kind: Pod
spec:
  securityContext:
    runAsUser: 1000
  containers:
  - name: my-container
    image: my-image
    command: ["sleep", "3600"]
```
or
```yaml
...
kind: Pod
spec:
  containers:
  - name: my-container
    image: my-image
    command: ["sleep", "3600"]
    securityContext:
        runAsUser: 1000
        capabilities:
          add: ["NET_ADMIN"]

```



---
