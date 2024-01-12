# Backup

- Resource configurations
- ETCD
- Persistent volumes

### Resource definition files
 - upload to Source code manager (Github)

From apiserver
```
kubectl get all -A -o yaml > all-services.yaml
```
- Tool: Velero by HeptIO 

### ETCD
can backup this instead of all resource 
configs

data dir of the etcd can be backed up
- Snapshot feature available


Cluster info with
```
kubectl config get-clusters
```
To view which cluster is currently used
```
kubectl config current-context
```
To switch to another cluster
```
kubectl config use-context <clustername>
```
ssh into cluster and run ps -ef to get info on servers running