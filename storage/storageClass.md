
## Storage Class

shorthand- sc

### Static Provisioning:

1. Provision a block of storage, a disk/cloud storage
2. Create a PV with the storage provisioned
3. Create a PVC 
4. Attach the PVC to the Pod

### Dynamic Provisioning:

1. Create a storage class object (specify where the storage is)
2. Create a PVC (with storage class name specified) - which makes the SC create a PV with storage/access needed by PVC
3. Attach the PVC to pod

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: aws-ebs
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
```

- Can use to create a defined set of different classes of storage type and other parameters

### Volume Binding Mode: WaitForFirstConsumer, Immediate(default)
- if Immediate - the pv is attached to pvc when the claim matches (even when the claim is bnot used by any pod)
- if this is WaitForFirstConsumer, the pv wont be created/attached to pvc until the claim is used by a pod.
