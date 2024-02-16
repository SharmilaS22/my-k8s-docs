## Daemon Sets
In cmd, `ds`
1 pod in each node
- to existing and new nodes that are added

Ignored by kube scheduler

usecases:
- Monitoring
- Logs

Eg, kubeproxy is needed in every node, which could be achieved by Daemon sets

```yaml
apiVersion: apps/v1
kind: DaemonSet
...(like replica set)
```

To create Daemon set,
create Deployment yaml, change kind to DaemonSet and remove replica line

