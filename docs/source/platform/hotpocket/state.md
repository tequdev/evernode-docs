# State

HotPocket dapps can persist data on to the disk via regular file system operations, hence it is preserved and accessible between different invocations of the contract. HotPocket subjects the persisted data to consensus so that majority of nodes are in agreement on the data that gets persisted on to their disks. The consensed data on disk is called the dapp "state" of the entire HotPocket cluster. When the dapp is invoked after a consensus round, it has read/write access to the consensed data on disk (state) via regular file system operations.

If a HotPocket node finds out that its state is "out of sync" with other nodes on the cluster, it will attempt to synchronize its state with the consensed state of the cluster. In this case it will not invoke the dapp until its state is in sync with the cluster.

_**dapp binary in "state":** Although not required to do so, it is desirable for the dapp software itself to reside on "state". This ensures the application files of the dapp are also subjected to consensus, guaranteeing that each HotPocket node is hosting the same dapp version/logic._