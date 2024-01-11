## Static pods

When there is no master node(no apiserver, scheduler or any control plane component)

Ignored by kube scheduler

A standalone node - which has kubelet - can create and manage pods by

- Creating and storing pod definition file in a _directory_ for pod information in node.

These are called Static pods. No other objects like deployment, replica sets etc cant be created

this option for kubelet service is used to set directory path
`--pod-manifest-path`
or can provide a config file 
`--config=kubeconfig.yaml`

_kubeconfig.yaml_
```
staticPodPath: /path/to/static-pod-dir/
```

To view pods(containers),
use `docker ps` (since no api server)

Kubelet can create both static pods and pods from api server at the same time.

API Server will also know about the static pod.
A mirror object (read-only) of this static pod is  created by kube apiserver, which can be accessed by kubectl.

This method can be used to create a master node with _control planes components_ as static pod using kubelet and definition files.

The name of the static pod ends with node name as suffix

---
To get info on kubelet or statis Pod path

ssh into the node
```
ssh <node-name>
 or
ssh <node-ip>
```
run
```
systemctl cat kubelet
```
- gives all info on kubelet including the kube-config file location
- check the config file for staticPodPath
- go to the path, and add defnition files etc

---


