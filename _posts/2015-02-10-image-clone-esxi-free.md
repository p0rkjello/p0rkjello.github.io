---
layout: post
title: 'Image cloning in VMware ESX shell'
author: 'Andrew Bounds'
support: true
comments: true
tags: ['vmware', 'esxi']
---

## VMware image clone via ESX shell and vmkfstools

The free version of the VMware vSphere Hypervisor has a reduced feature set. One of the missing features is the ability to clone or create
templates from existing vms. You can clone an image using `vmkfstools` within the ESX shell.

## vmkfstools

```console
OPTIONS FOR FILE SYSTEMS:
vmkfstools -C --createfs [vmfs3|vmfs5]
               -b --blocksize #[mMkK]
               -S --setfsname fsName
           -Z --spanfs span-partition
           -G --growfs grown-partition
   deviceName
           -P --queryfs -h --humanreadable
           -T --upgradevmfs
   vmfsPath
           -y --reclaimBlocks vmfsPath [--reclaimBlocksUnit #blocks]
OPTIONS FOR VIRTUAL DISKS:
vmkfstools -c --createvirtualdisk #[gGmMkK]
               -d --diskformat [zeroedthick
                               |thin
                               |eagerzeroedthick
                               ]
               -a --adaptertype [buslogic|lsilogic|ide
                                |lsisas|pvscsi]
               -W --objecttype [file|vsan]
               --policyFile <fileName>
           -w --writezeros
           -j --inflatedisk
           -k --eagerzero
           -K --punchzero
           -U --deletevirtualdisk
           -E --renamevirtualdisk srcDisk
           -i --clonevirtualdisk srcDisk
               -d --diskformat [zeroedthick
                               |thin
                               |eagerzeroedthick
                               |rdm:<device>|rdmp:<device>
                               |2gbsparse]
               -W --object [file|vsan]
               --policyFile <fileName>
               -N --avoidnativeclone
           -X --extendvirtualdisk #[gGmMkK]
               [-d --diskformat eagerzeroedthick]
           -M --migratevirtualdisk
           -r --createrdm /vmfs/devices/disks/...
           -q --queryrdm
           -z --createrdmpassthru /vmfs/devices/disks/...
           -v --verbose #
           -g --geometry
           -x --fix [check|repair]
           -e --chainConsistent
           -Q --objecttype name/value pair
           --uniqueblocks childDisk
   vmfsPath
OPTIONS FOR DEVICES:
           -L --lock [reserve|release|lunreset|targetreset|busreset|readkeys|readresv
                     ] /vmfs/devices/disks/...
           -B --breaklock /vmfs/devices/disks/...
vmkfstools -H --help
```

## Identify source image and destination location

Log into the ESX sever and identify the location of the source image. In
this example I will be cloning image `centos-0` on `datastore-1`

```shell
cd vmfs/volumes/datastore-1/
/vmfs/volumes/datastore-1 # ls
centos-0
```

I am going to create 3 additional CentOS vm images. Create the
destination directory/s prior to running `vmkfstools`.

```shell
/vmfs/volumes/datastore-1 # mkdir centos-1 centos-2 centos-3
/vmfs/volumes/datastore-1 # ls
centos-0  centos-1  centos-2  centos-3
```

Clone the image via `vmkfstools -i source destination`. If not specified
the new image will be *thick* provisioned. The first will use the
`vmkfstools` command without additional options. The 2nd and 3rd will
`thin` provision.

***Thin***

> These virtual disks do not reserve space on the VMFS
> filesystem, nor do they reserve space on the back-end storage. They
> only consume blocks when data is written to disk from within the
> VM/Guest OS. The amount of actual space consumed by the VMDK starts
> out small, but grows in size as the Guest OS commits more I/O to disk,
> up to a maximum size set at VMDK creation time. The Guest OS believes
> that it has the maximum disk size available to it as storage space
> from the start.

***Thick (aka LazyZeroedThick)***

> These disks reserve space on the VMFS
> filesystem but there is an interesting caveat. Although they are
> called thick disks, they behave similar to thinly provisioned disks.
> Disk blocks are only used on the back-end (array) when they get
> written to inside in the VM/Guest OS. Again, the Guest OS inside this
> VM thinks it has this maximum size from the start.

***EagerZeroedThick***

> These virtual disks reserve space on the VMFS
> filesystem and zero out the disk blocks at creation time. This disk
> type may take a little longer to create as it zeroes out the blocks,
> but its performance should be optimal from deployment time (no
> overhead in zeroing out disk blocks on-demand, meaning no latency
> incurred from the zeroing operation). However, if the array supports
> the VAAI Zero primitive which offloads the zero operation to the
> array, then the additional time to create the zeroed out VMDK should
> be minimal.

```shell
/vmfs/volumes/datastore-1 # vmkfstools -i centos-0/centos-0.vmdk centos-1/centos-1.vmdk
Destination disk format: VMFS zeroedthick
Cloning disk 'centos-0/centos-0.vmdk'...
Clone: 100% done.

/vmfs/volumes/datastore-1 # ls -la centos-1/
total 16778248
drwxr-xr-x    1 root     root           560 Feb  3 18:27 .
drwxr-xr-t    1 root     root          1820 Feb  3 18:17 ..
-rw-------    1 root     root     17179869184 Feb  3 18:25 centos-1-flat.vmdk
-rw-------    1 root     root           524 Feb  3 18:27 centos-1.vmdk
```

Clone a 2nd copy to the centos-2 directory. Specify `-d thin` to *thin*
provision this image.

```shell
/vmfs/volumes/datastore-1 # vmkfstools -i centos-0/centos-0.vmdk centos-2/centos-2.vmdk -d thin
Destination disk format: VMFS thin-provisioned
Cloning disk 'centos-0/centos-0.vmdk'...
Clone: 100% done.
/vmfs/volumes/datastore-1 # ls -la centos-2/
total 1471496
drwxr-xr-x    1 root     root           560 Feb  3 18:36 .
drwxr-xr-t    1 root     root          1820 Feb  3 18:17   ..
-rw-------    1 root     root     17179869184 Feb  3 18:34 centos-2-flat.vmdk
-rw-------    1 root     root           550 Feb  3 18:36 centos-2.vmdk
```

Clone a 3rd copy to the centos-3 directory

```shell
/vmfs/volumes/datastore-1 # vmkfstools -i centos-0/centos-0.vmdk centos-3/centos-3.vmdk -d thin
Destination disk format: VMFS thin-provisioned
Cloning disk 'centos-0/centos-0.vmdk'...
Clone: 100% done.
/vmfs/volumes/datastore-1 # ls -la centos-3/
total 1471496
drwxr-xr-x    1 root     root           560 Feb  3 18:42 .
drwxr-xr-t    1 root     root          1820 Feb  3 18:17 ..
-rw-------    1 root     root     17179869184 Feb  3 18:40 centos-3-flat.vmdk
-rw-------    1 root     root           550 Feb  3 18:42 centos-3.vmdk
```

3 new copies of the CentOS vmdk files have been created. The
`vmkfstools` command does not copy any of the additional files from
within the source directory. I will add some details on this at a later
time. For this example the cloned images will be added via the vSphere
Client.

## Add the newly cloned images to the vSphere inventory.

Log into the **vSphere Client** and add a **New Virtual Machine**
(Ctrl-N).

Step through the **New Virtual Machine Wizard** taking note of
**Configuration** and **Select a Disk** pages. Select *Custom* from the
Configuration step of the Configuration wizard.
![vsphere-config-custom](/assets/img/vsphere-config-custom.png)

Select *Use an existing virtual disk.* from the Select a Disk step of
the Configuration wizrd.
![vsphere-config-existing](/assets/img/vsphere-config-existing.png)
