
```
kubectl logs -f <pod-name>
```
-f to live stream logs


Incase of multiple containers, to filter one container logs
```
kubectl logs -f <pod-name> <container-name>
```