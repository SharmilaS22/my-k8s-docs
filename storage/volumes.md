

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: nginx
      volumeMounts:
        - name: data-volume
          mountPath: /data
  volumes:
    - name: data-volume
      emptyDir: {} # exist for the lifetime of the pod
    - name: data-on-host
      hostPath: # on the node, lives even after pod goes down
        path: /data
        type: Directory
    - name: data-ebs # from the cloud
      awsElasticBlockStore: 
        volumeID: <vol-id-ebs>
        fsType: ext4
    - name: data-pvc # use pvc to get volume
      persistentVolumeClaim:
        claimName: my-pvc

```

The hostpath is not recomended, since in multiple nodes cluster, the data is not synced.

## Persistent Volumes

Centralised data volume, on which a part can be used by each pod by raising a PersistentVolumeClaim with storage and access needed.

PV and PVC has 1:1 relationship, 
PVC is bound to one PV,  chosen by matching the storage and access claims



```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain # Retain/Delete/Recycle
  storageClassName: standard
  hostPath: # where the pv uses the storage
    # for prod use centralised data store (like ebs etc)
    p: "/mnt/data"

---

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: standard
  resources:
    requests:
      storage: 1Gi
```