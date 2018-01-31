# NFS Provisioner

 The [NFS provisioner](https://github.com/kubernetes-incubator/external-storage/tree/master/nfs) is an out-of-tree/external storage provisioner for Kubernetes. It provides dynamic provisioning of Persistent Volumes from a single NFS mount point.

 It works just like in-tree dynamic provisioners: a `StorageClass` object can specify an instance of NFS Provisioner to be its `provisioner` like it specifies in-tree provisioners such as GCE or AWS. Then, the instance of NFS Provisioner will watch for `PersistentVolumeClaims` that ask for the `StorageClass` and automatically create NFS-backed `PersistentVolumes` for them. For more information on how dynamic provisioning works, see [the docs](http://kubernetes.io/docs/user-guide/persistent-volumes/).
## TL;DR;

 ```console
 $ helm install --name nfs-provisioner --namespace storage dlc/nfs-provisioner
 ```

## Introduction

 This charts installs [NFS provisioner](https://github.com/kubernetes-incubator/external-storage/tree/master/nfs) and a custom [storage class](https://kubernetes.io/docs/concepts/storage/storage-classes/) into a [Kubernetes](http://kubernetes.io) cluster

## Prerequisites

 - Kubernetes 1.7+

## Installing the Chart

 To install the chart with the release name `nfs-provisioner`:

 ```console
 $ helm install --name nfs-provisioner --namespace storage dlc/nfs-provisioner
 ```

 The command deploys NFS Provisioner on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

 > **Tip**: List all releases using `helm list`

## Uninstalling the Chart

 To uninstall/delete the `nfs-provisioner` release:

 ```console
 $ helm delete --purge nfs-provisioner
 ```

 The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

 The following tables lists the configurable parameters of the NFS Provisioner chart and their default values.

 | Parameter               | Description                           | Default                                                    |
 | ----------------------- | ----------------------------------    | ---------------------------------------------------------- |
 | `image.repository` | NFS Provisioner image | `quay.io/kubernetes_incubator/nfs-provisioner` |
 | `image.tag` | NFS Provisioner image version | `v1.0.9` |
 | `image.pullPolicy` | NFS Provisioner image pull policy |  `IfNotPresent` |
 | `persistence.enabled` | Enable NFS Provisioner persistence using Persistent Volume Claims | `false` |
 | `persistence.storageClass` | NFS Provisioner Persistent Volume Storage Class | `default` |
 | `persistence.accessMode` | NFS Provisioner Persistent Volume Access Mode | `ReadWriteOnce` |
 | `persistence.size` | NFS Provisioner Persistent Volume Storage Size | `1Gi` |
 | `resources.request.memory` | NFS Provisioner memory request | `128Mi` |
 | `resources.request.cpu` | NFS Provisioner cpu request | `100m` |
 | `resources.limits.memory` | NFS Provisioner memory limit | `521Mi` |
 | `resources.limits.cpu` | NFS Provisioner cpu limit | `400m` |

 The configurable parameters of the NFS Provisioner  chart and the default values are listed in `values.yaml`.

 Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example to enable persistence,

 ```bash
 $ helm install --name nfs-provisioner --namespace storage dlc/nfs-provisioner \
   --set persistence.enabled=true
 ```

## Persistence

 The [NFS provisioner](https://github.com/kubernetes-incubator/external-storage/tree/master/nfs) image stores data in the `/export` directory in the container.

 The chart optionally mounts a [Persistent Volume](http://kubernetes.io/docs/user-guide/persistent-volumes/) volume at this location. The volume is created using dynamic volume provisioning.
