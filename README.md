## Rook-spike

Manage kubernetes storage using [Rook](https://www.rook.io).
This creates a [Ceph](https://ceph.com/) cluster using filesystems on the
kubernetes nodes (not the masters), and then divides it up into pools to provide to pods that need storage.
The main use-case is for bare-metal environments with no simple StorageClass options.

0. Prepare nodes with empty filesystems mounted at /opt/rook/data for the ceph cluster to use. Rook metadata
will be stored at /var/lib/rook, but this directory will be automatically created.
1. Run rook-operator, rook-cluster and rook-storageclass yamls into your k8s cluster. Note that if you are
not using Kubespray, you may need to change env var FLEXVOLUME_DIR_PATH to match your installation, or remove it.
Also node that this will set the Rook storageclass as the default storageclass. If you run rook-storageclass.yaml on
a kubernetes cluster that already has a default storageclass, the annotation will confuse kubernetes, and as a result
there will be no default storageclass any more.
2. Any statefulsets that were waiting for a default StorageClass will need to be deleted and re-created, along with
their PVCs.