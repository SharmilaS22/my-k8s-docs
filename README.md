# My k8s notes


Resources:
- CKA course on Udemy by Mumshad
---

Important Links:
- https://kubernetes.io/docs/reference/kubectl/cheatsheet
- https://discuss.kubernetes.io
- https://kubernetes.io/docs/home/
---

Docs:
- [Namespaces](Resources/namespaces.md)
- [Imperative commands](Resources/imperative-cmds.md)
### Scheduling:
- [Basics](Scheduling/basics.md)
- [Taints and Tolerations](Scheduling/taints&Tolerations.md)
- [Node Selectors and Node Affinity](Scheduling/nodeAffinity.md)
- [Resource Limits](Scheduling/resourceLimits.md)
- [Daemon Set](Scheduling/DaemonSets.md)
- [Static pod](Scheduling/staticPods.md)
- [multipleSchedulers]()

### [Monitoring](Logging/MonitorClusterComp.md) and [Logging](Logging/ManageAppLogs.md)

### App Lifecycle Management
- [Arguments and Commands](AppLifecycle/arguments.md)
- [Rolling updates](AppLifecycle/Rollingupdates.md)

## Cluster Maintainance
- [Backup](./ClusterMaintainance/backup.md)
- [OS Upgrades](./ClusterMaintainance/OS_Upgrades.md)

## Security
- [authentication](./Security/auth.md) 
- [tls](./Security/tls.md)
- [authorization](./Security/authorization.md)
- [apiGroups](./Security/apiGroups.md)
- [Service accounts](./Security/serviceAccounts.md)
- [private registry images](./Security/privateImage.md)
- [network Policy](./Security/networkpolicy.yaml)