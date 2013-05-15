### Current status
Docker has volumes which can be reused from container to container. These volumes have the following limitations:
* they can't be imported and exported (without piping data into and out of the container)
* they can't be backed up and restored from backups (without piping data into and out of the container)
* they can't be stored on custom storage (e.g.: use high IOPS storage for a volume and regular IOPS storage for others)
* they can't be used for exposing data from the host into the container during runtime
* it's not possible to cherry pick the containers to be used from an old container in a new container
* it's not possible to manage the volumes after deleting the containers to which they were attached

***

### Desired features
Volumes should be extended so that they become more useful. Some of these features may not integrate properly into the general vision of how docker should work, so feedback is welcome.

**Backups**

It should be possible to back up a volume and restore it from a backup. These can either reuse the docker export paradigm (export as tar via stdout, import tar via stdin) or docker could manage the backups as a new type of entity with full history.
The full history backups could enable cool features (graph of backups).
Backups should keep the metadata of the volumes.

**Exposing volumes to the host**

It should be possible to allow the host to perform all sorts of operations on the volume when needed.
For example, we could bind mount a volume from its location to a custom location on the host even when a container is running.
example:
```
docker bind-volume <container_id> /container/volume/as/path /host/location/where/we/bind/mount/to
container volume remains where docker stores it
docker unbind-volume /host/location/where/we/bind/mount/to
```

**Import and export**

It should be possible to import and export volumes. These could be a part of the backups feature if backups simply get read via stdin and written via stdout.
These operations should preserve the metadata as well.


**Custom allocation of volumes from storage pools**

It should be possible to pick the location where a volume should be stored. This could be done via storage pools. This would allow docker to keep track of the volume data, even though we're now storing it on different backing storage.
This feature wouldn't let docker simply allocate volumes at arbitrary host locations. Instead, named storage pools are defined and volumes are allocated in those storage pools.
example:
```
#define the storage pool location
docker add-pool dbstore /mnt/dbssd
#docker will now keep track of all volumes allocated under this directory
#a special marker file will also be added to identify this as a docker storage pool
#run docker with a volume allocated on dbstore
docker run -d -v dbstore:/var/lib/mydata ubuntu bash
#we're now running bash in a container with a volume allocated on dbstore
```

**Named volumes**

Volumes might store extremely important data, such as the data of a database. We should be able to name them and use the named volumes in new containers. We should be able to keep track of the volumes, delete them and so on.
This has to be implemented in order to make volumes more reliable.

**Cherry picking volumes to be reused**

A container might have multiple volumes attached and not all of them might be storing data which is of any use beyond the execution of the container to which they were added the first time.
We should be able to specify the exact volume(s) to be reused from an old container in a new one.
