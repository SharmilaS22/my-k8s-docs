## Docker Networks

```sh
docker network ls

# - None
# - Bridge
# - Host
```

- None - No networking with any container or the host
- Host - Deployed in host network, can directly access from host with port
- Bridge - similar with virtual switch connecting network namespaces
    - docker creates a vswitch, all containers are connected to this bridge/switch - so they can communicate with each other.
    - to access from the host, port forwarding `-p 8080:8080` can be done.
