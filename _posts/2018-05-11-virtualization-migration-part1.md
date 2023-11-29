---
layout:    post
title:    Virtualization Migration - Part 1
date:    2018-05-30
categories: virtualization
---

Virtualization revolutionized the datacenter by increasing the utilization of existing hardware and reduced datacenter server footprints.  Today many customers that have virtualized environments are now needing to replace their existing virtualization platforms due to cost because of agreements that include products and features that they don't use or need.  Open Source alternatives to proprietary virtualization platforms such as Red Hat Virtualization can meet enterprise needs.  Enterprise-class features such as live migration, storage live migration, and resource schedulers that automatically handle load balancing and affinity/anti-affinity rules are now table stakes features that Red Hat Virtualization and oVirt have and much more.  

Migrating to another platform in an enterprise is not easy and takes a lot of time and process.  The technology is the easy part.  In this post, I will be discussing the tools used to get from one platform to another.

Many tools can be used to convert virtual disks so they can run on different platforms.  In this post, I will be addressing two, qemu-img and virt-v2v.  These two have gained notable acceptance and adoption in the open source community.


## QEMU Tools

QEMU is a hardware emulator project. It is used as part of both the KVM and XEN hypervisor.  QEMU has the tooling to convert disks to many different formats.  The qemu-img tool can convert images to and from RAW, QCOW2, VMDK (VMware), VHDX (HyperV), and VDI (VirtualBox). 

For example, if I want to convert a VirtualBox image to QCOW2 which I did a lot of when migrated of Mac back to Linux.

```
qemu-img convert -f vdi -O qcow2 \
/home/virtualbox-images/vdi-disk-image.vdi \
/home/qcow-images/converted-vdi-image.qcow2
```

While qemu-img can convert to most disk image formats that is all it can do.  Before converting a disk from VirtualBox to KVM, it might be a good idea to ensure that all the needed drivers present for KVM such as virt-io drivers for vNics and virtual disks.  It is also a good idea that the unneeded software like the VirtualBox tools is uninstalled.  In my opinion, qemu-img is excellent for that one-off disk that you want to convert


## Virt-V2V part of the libguestfs tools
Straight from the description of the libguestfs.org website, "libguestfs is a set of tools for accessing and modifying virtual machine disk images.  You can use this for viewing and editing files inside guests, scripting changes to VMs, monitoring disk used/free statistics, creating guests, P2V, V2V, and performing backups, cloning VMs, building VMs, formatting disks, resizing disks and much more."

The libguestfs tools are my favorite of all the virtual disk manipulation tools out there because they are flexible.  The utility that I use the most from the suite of libguestfs is virt-v2v, which converts virtual machine disk that you can use with KVM.  What makes it so powerful is when it converts a VMDK from VMware it uninstalls the VMware tools and installs any needed drivers and tools for the target KVM platform.  The virt-v2v tool can take input directly from a VMware environment or convert OVAs exported from VMware.  Virt-v2v can push disks to various targets such as Red Hat Virtualization, OpenStack Glance, libvirtd managed KVM or convert straight to raw or qcow2 disks.  Virt-V2V can also move VMs from libvirtd managed Xen and KVM servers to an oVirt/RHV managed environment.  Virt-v2v requires virtual machines to be powered off before converting.



Here are a few examples of virt-v2v:

### Converting VMware VM and write the virtual disk to a local file system

Sample Commands:
```
virt-v2v -ic vpx://vcenteruser@{vcenter_url}/{vcenter_datacenter}/{vcenter_cluster}/{vsphere_hosts} {vmware_vm} \
--password-file={file_containing_vcenterpassword} -o {target_system} -os {target_storage_path} \
-oa {sparse,preprovisioned} -of {target_format}
```

Command I used:
```
virt-v2v -ic vpx://vsphere.local%5cadministrator@vcenter.open-tec.net/Datacenter/Default/vsphere1.open-tec.net?no_verify=1 centos7 \
--password-file=/home/brandon/vmwarepassword.txt -o local -os ~/VMConvert/ToQCOW/ -oa sparse -of qcow2
```


So let's break down the command -ic is the target you want to pull from.  In this example, the URI for VMware using the libvirtd syntax.  The first part of the URI vsphere.local is the default "domain" in a default vCenter install and you need to pass a "/" and it is passed with the '%5C'.  The rest of the syntax is pretty straight forward.  Administrator for the user and the vCenter that you want to migrate VMs from as well as the folder and finally the ESXi hypervisor that the VM is running on.  I like to use the password files rather than type my password, it makes it easier to automate the process.  The only line in the file should be the VMware password.  The next part of the command is where the disk is going and what kind of disk you want to convert too.

Virt-v2v creates two files, first the virtual disk and an XML file.  For every disk, virt-v2v creates a separate disk image and names it based on the order the disk is on the virtual bus; for example, centos7-sda, centos7-sdb, etc. The second file is a libvirt descriptor.  From here you can now start up the  VM for import it into your KVM storage pool and modify the generated XML to reflect that change and start up the VM.

Commands to upload the disk to the libvirt data pool
```
size=$(stat -c%s ~/VMConvert/ToQCOW/centos7-sda)
virsh vol-create-as VirtualMachines centos7-sda $size --format raw
virsh vol-upload --pool VirtualMachines centos7-sda ~/VMConvert/ToQCOW/centos-sda
```

You could start this VM now from where it is.  I prefer to keep everything tidy, so that is why I upload the disk to the storage pool, so to do that I'm going to modify the generated XML.

centos7.xml orignal
```
<?xml version='1.0' encoding='utf-8'?>
<domain type='kvm'>
  <!-- generated by virt-v2v 1.38.0fedora=27,release=1.fc27,libvirt -->
  <name>centos7</name>
  <memory unit='KiB'>2097152</memory>
  <currentMemory unit='KiB'>2097152</currentMemory>
  <vcpu>1</vcpu>
  <features>
    <acpi/>
    <apic/>
  </features>
  <os>
    <type arch='x86_64'>hvm</type>
  </os>
  <on_poweroff>destroy</on_poweroff>
  <on_reboot>restart</on_reboot>
  <on_crash>restart</on_crash>
   <devices>
    <disk type='file' device='disk'>
      <driver name='qemu' type='qcow2' cache='none'/>
      <source file='/home/brandon/VMConvert/ToQCOW/centos7-sda'/>
      <target dev='vda' bus='virtio'/>
    </disk>
    <interface type='bridge'>
      <source bridge='VM Network'/>
      <model type='virtio'/>
      <mac address='00:50:56:ab:68:b9'/>
    </interface>
    <video>
      <model type='qxl' ram='65536' heads='1'/>
    </video>
    <graphics type='vnc' autoport='yes' port='-1'/>
    <rng model='virtio'>
      <backend model='random'>/dev/urandom</backend>
    </rng>
    <memballoon model='virtio'/>
    <panic model='isa'>
      <address type='isa' iobase='0x505'/>
    </panic>
    <input type='tablet' bus='usb'/>
    <input type='mouse' bus='ps2'/>
    <console type='pty'/>
  </devices>
</domain>
```

I changed the disk type from file to volume and provided it with the storage pool path.  I also changed the network name to the Linux bridge name in my environment.
centos7.xml modified
```
<?xml version='1.0' encoding='utf-8'?>
<domain type='kvm'>
  <!-- generated by virt-v2v 1.38.0fedora=27,release=1.fc27,libvirt -->
  <name>centos7</name>
  <memory unit='KiB'>2097152</memory>
  <currentMemory unit='KiB'>2097152</currentMemory>
  <vcpu>1</vcpu>
  <features>
    <acpi/>
    <apic/>
  </features>
  <os>
    <type arch='x86_64'>hvm</type>
  </os>
  <on_poweroff>destroy</on_poweroff>
  <on_reboot>restart</on_reboot>
  <on_crash>restart</on_crash>
  <devices>
    <disk type='volume' device='disk'>
      <driver name='qemu' type='qcow2' cache='none'/>
      <source pool='VirtualMachines' volume='centos7-sda'/>
      <target dev='vda' bus='virtio'/>
    </disk>
    <interface type='bridge'>
      <source bridge='br1'/>
      <model type='virtio'/>
      <mac address='00:50:56:ab:68:b9'/>
    </interface>
    <video>
      <model type='qxl' ram='65536' heads='1'/>
    </video>
    <graphics type='vnc' autoport='yes' port='-1'/>
    <rng model='virtio'>
      <backend model='random'>/dev/urandom</backend>
    </rng>
    <memballoon model='virtio'/>
    <panic model='isa'>
      <address type='isa' iobase='0x505'/>
    </panic>
    <input type='tablet' bus='usb'/>
    <input type='mouse' bus='ps2'/>
    <console type='pty'/>
  </devices>
</domain>
```

After you change the XML to reflect the data pool define the guest in libvirt
```
virsh define ~/VMConvert/ToQCOW/centos7.xml
```

Start the VM
```
virsh start centos7
```
Here is the video of this entire process, in the video I use the sed command to modify the centos7.xml file:
[![asciicast](https://asciinema.org/a/oFf28Ujp6JYU3QnDyhUasbpXg.png)](https://asciinema.org/a/oFf28Ujp6JYU3QnDyhUasbpXg)



#### VMware managed by vCenter converting to RHV
This feature is in the latest version of Virt-V2V that doesn't ship with Fedora or CentOS 7 at the time of this writing.  The command below should work with a newer version. 

```
virt-v2v -ic vpx://vsphere.local%5cadministrator@vcenter.open-tec.net/Datacenter/Default/vsphere1.open-tec.net?no_verify centos7 \
-o rhv-upload -oc https://ovirt-engine.open-tec.net/ovirt-engine/api -os Data \
-op /home/brandon/ovirt-admin-password -of raw -oo rhv-cafile=/home/brandon/ca.pem -oo rhv-direct --bridge ovirtmgmt
```