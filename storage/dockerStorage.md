
- Storage drivers
- Network drivers


File system
```sh
/var/lib/docker
    /images
    /volumes
    /container
    ...
```

Container layer is a read-write layer on top of the image(which has many layers - read only)

Any information on files/logs created/modified when the container is running is only stored in this container layer. (The image layers are not altered)

To persist this data - will use volumes, 
- can create docker voln and mount it to a location in the container (Volume mount)
- can mount a directory from any location in the host to a location in the container (Bind mount)

```sh
# Volume mount
$ docker run -v \
<docker_data_volume_name>:<path/in/the/container> <image-name>

$ docker run -v \
data_voln_1:/var/lib/mysql \
mysql

# Bind mount
$ docker run -v \
<path/in/the/host>:<path/in/the/container> <image-name>

$ docker run -v \
/data/mysql:/var/lib/mysql \
mysql

# using mount option instead of -v
$ docker run  \
--mount type=bind,source=/data/mysql,target=/var/lib/mysql \
mysql
```
[Mount vs v option docs](https://docs.docker.com/storage/bind-mounts/#choose-the--v-or---mount-flag)

---

Storage Drivers: AUFS, ZFS, BTRFS, OVERLAY etc

Network Drivers: drivers for volumes like aws ebs, azure file storage, local, 

Can use appropriate network drivers for storage volumes. (like rexrey/ebs for aws ebs)

---

## Container Storage Interface (CSI)
[ref](https://github.com/container-storage-interface/spec)

It's a standard that allows the different Container Orchestration tools(k8s etc)  to use different storage providers(ebs, file storage etc)


