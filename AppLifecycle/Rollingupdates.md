## Rolling updates

To rollout new revision,
```sh
kubectl rollout status deployment <deployment-name>
```
To view history of rollout
```sh
kubectl rollout history deployment <deployment-name>
```
To change image of the container (maybe new versions), IMPERATIVELY
```sh
kubectl set image deployment <deployment-name> <container-name>=<new-image-name>
```
Deployment stratrgy - Default - Rolling update
1.  Recreate - App goes down for some time
   - Destroy old instances
   - Create new instances (w/ change)
2. Rolling - App's up all the time
   - Replace old instances with new one-by-one.

Rollback
```sh
kubectl rollout undo deployment <deploy-name>
```

