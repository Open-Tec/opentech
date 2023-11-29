---
layout:	post
title: RHV Automated Operations
date:	2018-06-22
categories: virtualization, automation
---

One of the best enhancements of Red Hat Virtualization is the inclusion of several ansible roles to make operating a RHV environment at scale much easier.

#### Installation Roles:
* [oVirt.engine-setup](https://github.com/oVirt/ovirt-ansible-engine-setup) - Setup and upgrade the RHV Manager (oVirt Engine)

* [oVirt.repositories](https://github.com/oVirt/ovirt-ansible-repositories)

* [oVirt.infra](https://github.com/oVirt/ovirt-ansible-infra) - Configure a RHV enviornment including Datacenters, Clusters, Storage, and Networks


#### Operation Roles:
* [oVirt.cluster-upgrade](https://github.com/oVirt/ovirt-ansible-cluster-upgrade) - Performs a Rolling upgrade of specifed hosts and clusters
* [oVirt.disaster-recovery](https://github.com/oVirt/ovirt-ansible-disaster-recovery) - Sets up disaster recovery and failover and fallback 
* [oVirt.vm-infra](https://github.com/oVirt/ovirt-ansible-vm-infra) - Role to setup and configure virtual machines
* [oVirt.image-template](https://github.com/oVirt/ovirt-ansible-image-template) - Roles to Manage Templates and create VMs from Templates
* [oVirt.manageiq](https://github.com/oVirt/ovirt-ansible-manageiq) - Roles to setup ManageIQ and CloudForms
* [oVirt.v2v-conversion-host](https://github.com/oVirt/ovirt-ansible-v2v-conversion-host) - Configure a host as a VM conversion host for ManageIQ/CloudForms

In this series I will review the operations roles and how to use them to automate RHV/oVirt operations, such as upgrades disaster recovery, VM deployment, managing a CloudForms deployment.  In part one I will go over performing a rolling upgrade of the RHV hypervisors using the oVirt.cluster-upgrade Ansible role and upgrading the RHV Manager using the oVirt.engine-setup role.

Before we get into the upgrade process lets talk installation of RHV.  RHV can be deployed several different ways.  The RHV Hypervisors can be full RHEL systems or they can be a small foot print hypervisors called RHV-H (upstream oVirt Node).  There are advantages and disadvantages to both.  Linux admins that are use to managing traditional RHEL may prefer a full RHEL install, the down side is the size of the install.  A RHEL install for a virtualization host is about 800 MB.  RHV-H is managed in a similar fashion to ESXi.  It is managed using a UI console based on cockpit.  Cockpit can be used to configure networking and more importantly used to install the RHV Manager.  RHV Manager can be installed via cockpit, a script or an Ansible playbook.  Using the playbook method makes upgrades easier down the road.

Example Playbook that I'm using in Ansible Tower:

```
---
- name: oVirt cluster upgrade
  gather_facts: false

  vars:
    engine_url: '{{ ovirt_engine }}'
    engine_user: '{{ ovirt_user }}'

    cluster_name: '{{ ovirt_cluster }}'
    stop_non_migratable_vms: true
    host_statuses:
      - up
    host_names: '{{ ovirt_host }}'

  roles:
    - oVirt.cluster-upgrade

```

This simple playbook will cycle through all the hosts in a cluster, one at a time.  It will shut off VMs that can't be migrated.  Those VMs could be systems that can't be migrated because they are host bound because they are attached to a PCI device or the affinity/anti-affinity rules prevent the migration.  After the VMs are powered off or migrated to other hosts the hypervisor is put into maintenance mode upgraded and rebooted.  After the hypervisor is reactivated in the cluster it moves on to the next listed host in the cluster.

Right now my main challenge is my second cluster which is hyperconverged.  Before you can move on to the next host in the cluster, Gluster needs to heal the volumes.  Right now I just run a similar play but specify only one host and perform the gluster heal command and then wait for 30 minutes before moving on to the next host.  This isn't ideal I'd rather it check the RHV API for status of the volumes and then move on to the next host as soon as the heal task is complete.













