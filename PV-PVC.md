# Persistent Volumes and Persistent Volume Claims

## Introduction

In OpenShift, Persistent Volumes (PVs) and Persistent Volume Claims (PVCs) are used to manage storage resources for pods. PVs are created and managed by administrators, while PVCs are used by applications to request and bind to these storage resources.

## Concepts

### Persistent Volume (PV)
A PV is a piece of storage in the cluster that has been provisioned by an administrator. It represents a real storage resource, such as an NFS share, a block device, or cloud storage. They are defined with a set of attributes such as capacity, access modes, and storage class.


### Persistent Volume Claim (PVC)
A PVC is a request for storage by a user. It specifies the amount of storage and access mode required. PVCs are dynamically bound to available PVs that match the request. PVCs are created by users or automation workflows to request storage resources.

### Volume Mounts and Volumes in Deployment

- In the deployment configuration, you need to specify how PVCs are used by your application. This involves defining volume mounts and volumes.

- Volumes Section: Defines the volumes that should be made available to containers in a pod. This section includes references to PVCs.

- Volume Mounts Section: Specifies the paths within the container where the volumes should be mounted. This allows the application to access the storage resources.

## Summary

1. To create a PV
```sh
oc create -f example_objects/rover-pv.yaml
```

2. To create a PVC
```sh
oc create -f example_objects/rover-pvc.yaml
```

3. Deploy rover with volume mounts and volumes as defined in the below mentioned file
```sh
oc create -f example_objects/rover-vol-deployment.yaml
```

4. Above setup allows rovers to use persistent storage, ensuring data durability and reliability across pod restarts.


## References

- [PV-PVCs](https://docs.openshift.com/container-platform/4.16/storage/understanding-persistent-storage.html#persistent-volumes_understanding-persistent-storage)
