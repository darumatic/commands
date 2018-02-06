# Pre-requisites
1. glusterfs client. The installation depends on our system.
2. Glusterfs endpoint. Typically volumes will be distributed across many nodes (endpoints)
3. Glusterfs volume name.

# Mount glusterfs volume
```
sudo mount -t glusterfs 10.20.15.196:appdata5 /mnt/gc/
```
