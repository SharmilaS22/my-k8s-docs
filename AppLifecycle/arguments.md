Docker EntryPoint vs Command

A Container lives till the command is executed

dockerfile - ubuntu-sleep
```
FROM ubuntu
CMD ["sleep", "5"]
```
`docker run ubuntu-sleep`-> sleeps for 5 sec

`docker run ubuntu-sleep sleep 10` -> overrrides the cmd to `sleep 10`

With entrypoint
Can give the arguments in run time
```
FROM ubuntu
ENTRYPOINT ["sleep"]
CMD ["5"]    ## default value
```

`docker run ubuntu-sleep` -> sleeps for 5 sec

`docker run ubuntu-sleep 10` -> sleeps for 10 sec

in pod.yaml, override the default value of command (argument for entrypoint)
```yaml
containers:
  - image: ubuntu-sleep
    name: ubuntu-sleep
    args: ["10"]
```

To override ENTRYPOINT command
`docker run --entrypoint <cmd> ubuntu-sleep <arg>`

in pod.yaml
```yaml
containers:
  - image: ubuntu-sleep
    name: ubuntu-sleep
    command: ["sleep2.0"]  ## override ENTRYPOINT
    args: ["10"]           ## override CMD (arg)
```
---

# ENV

## Environment variables
Environment variables in docker,
```
docker run -e APP_COLOR=red webapp
```
In pod.yaml,
```yaml
containers:
- name: webapp
  ...
  env:
  - name: APP_COLOR
    value: red
```

## Config maps
- key-value pairs
- shorthand: cm

to create (imperative)
```
kubectl create configmap <config-name> --from-literal=<key>=<value></value>

kubectl create cm app-config --from-literal=APP_COLOR=blue
--from-literal=APP_VERSION=v1
...
```

ConfigMap.yaml
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data: 
  APP_COLOR: blue
  APP_VERSION: v1
  ...
```

Can manage set of key-value pairs(config) for separate apps separately.
eg, app-config, db-config, url-config etc

```
kubectl get configmaps
kubectl get cm

kubectl describe configmaps
```

Use config map in **pod.yaml**
```yaml
containers:
  - name: somename
    ...
    envFrom:
    - configMapRef:
        name: app-config
```

for Single env
```yaml
name: webapp-container
...
env:
- name: APP_COLOR
  valueFrom:
    configMapKeyRef:
      name: app-config
      key: APP_COLOR
```
for volume
```yaml
volumes:
- name: app-config-volume
  configMap:
    name: app-config
```
---

## Secrets

- key-value pairs
- encoded

to create (imperative)
```bash
kubectl create secret generic <secret-name> --from-literal=<key>=<value></value>

kubectl create secret generic app-secret --from-literal=DB_HOST=mysql
--from-literal=DB_PSW=1234
...

<<from-literal = will enocde and save with base64>>


kubectl create secret generic <secret-name> --from-file=<path/to/file>

kubectl create secret generic app-secret --from-file=secret.properties
```

secret.yaml
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
data: 
  DB_HOST: adjklaj==
  DB_PSW: mandla==    ## Encoded values
  ...
```
Encode using,
```
echo -n 'mysql' | base64
```
Decode using,
```
echo -n 'adjklaj==' | base64 -d
```

Link to pod:
```yaml
spec:
  containers:
  - name: webapp
    ...
    envFrom:
    - secretRef:
        name: app-secret
```

Single env
```yaml
containers:
  - name: webapp
    ...
    env:
      - name: DB_Host
        valueFrom:
          secretKeyRef:
            name: app-secret
            key: DB_HOST
```
Volume, -> The secrets would be created as a file with the key, and value as its file content.
```yaml
containers:
- name: webapp
  secret:
    secretName: app-secret
```

- DO NOT CHECK IN SECRET OBJECTS ALONG WITH THE CODE TO REPOSITORIES
- SECRETS ARE ONLY ENCODED AND ENCRYPTED
- SECRETS ARE ENCRYPTED IN ETCD(can be achieved by configuring encryption at rest)
-  Anyone who have access to the namespace (to create pod etc), will ALSO HAVE ACCESS TO SECRETS
- Consider 3rd party secret providers.

Other information on secrets:
- The secret is only sent to a node when any pod in it requires the secret.
- kubelet store secrets in temp space and not disk storage
- once the pod which is using secrets is deleted, the secrets copy is also removed from the node

Alternatives: Helm secrets. hashicorp vault

---

## Init Container
This container runs before our main container runs.
Usecase: while installing some other repo/dependencies for the app

InitContainers run one by one in sequential order.
Until all the initcontainers are successful the main container will not start. The pod keeps restarting.

```yaml
spec:
  containers:
  ...
  initContainers:
  - name...
```