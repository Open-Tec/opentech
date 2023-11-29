---
layout:    post
title:    Expand Virtual Disk on KVM
date:    2018-05-30
categories: virtualization
---

I recently needed to increase the size of a virtual disk, specifically on a Windows VM.  To my pleasant surprise, I was able to do it with a tool in one of my favorite projects LibGuestFS + qemu-img.  Even though I'm focusing on a Windows VM in this post, these procedures should work on Linux VMs as well.

You will need to make sure that libguestfs is installed on your system to proceed.  In this post, I used `virt-filesystem` and `virt-resize` from the libguestfs utilities and `qemu-img` from the qemu project.

To increase the disk size first, you need to create a new disk image that is the target size.  So if you need to double your 60 gig drive create a new qcow2 disk image that can support 120 gigs

```qemu-img create -f qcow2 windows_new.img 120G```

After you create the new disk image, you need to identify the partition on the old disk you want to expand.  To do that run the following command:

```
$ virt-filesystems -a windows10.qcow2 -l
Name       Type        VFS   Label            Size         Parent
/dev/sda1  filesystem  ntfs  System Reserved  104857600    -
/dev/sda2  filesystem  ntfs  -                63424149504  -
/dev/sda3  filesystem  ntfs  -                892338176    -
```

From the output on this particular disk, I see three partitions.  The first partition should not be touched the second partition looks like my C:\ drive, so that is the one I'm going to target.

The next command is going to copy the disk contents into the new disk and resize the C: partition.

```
$ virt-resize --expand /dev/sda2 windows10.qcow2 windows_new.img
[   0.0] Examining windows10.qcow2
**********

Summary of changes:

/dev/sda1: This partition will be left alone.

/dev/sda2: This partition will be resized from 59.1G to 119.1G.  The 
filesystem ntfs on /dev/sda2 will be expanded using the ‘ntfsresize’ 
method.

/dev/sda3: This partition will be left alone.

**********
[   3.5] Setting up initial partition table on windows_new.img
[   4.8] Copying /dev/sda1
[   5.8] Copying /dev/sda2
 100% ⟦▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒⟧ 00:00
[ 447.8] Copying /dev/sda3
 100% ⟦▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒⟧ 00:00
[ 453.8] Expanding /dev/sda2 using the ‘ntfsresize’ method

Resize operation completed with no errors.  Before deleting the old disk, 
carefully check that the resized disk boots and works correctly.
```

A few minutes later you should have a windows system with 120 Gig drive. The beauty is I didn't need to do anything inside of the Windows VM to do it.

