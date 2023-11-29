---
layout:	post
title:	Simple Cloud Management with Mist
date:	2020-08-25
categories: Cloud, Management, Mist, ManageIQ
---

I'm no stranger to Cloud Management platforms and their concepts.  For a good portion of my time with Red Hat I worked with one of our products called CloudForms which is based on the open source project ManageIQ.  ManageIQ is an awesome platform that is well documented and used by hundreds of Red Hat customers.  The big downside to ManageIQ is it is a complex platform and frankly most CMPs are very complex.  While I was doing research for the Sudo Show on the topic of open source cloud management, I came across an open source product called Mist.  I had never heard of it until I stumbled on the product page of the company behind it and to my delight it had a self-hosted open source version.

Mist says it can manage a frankly large list of public cloud providers including the big 4, AWS, Azure, GCP and IBM.  It also supports several of the smaller providers such as Digital Ocean, Packet, Linode, and Vultr and on premise providers such as VMware, OpenStack, and libvirt managed KVM.  I discovered a KVM host and Digital Ocean.  Setting up KVM was a snap and Digital Ocean was pretty easy as well (just get an API Token).  What I liked about it is Mist discovered and brought under management all my Digital Ocean and KVM virtual machines! The surprise of the day after I discovered digital ocean was the display of my monthly cost in the public cloud!  I wanted to see how complete that was and I discovered Azure and AWS.  I spun up a virtual machine through Mist in both AWS and Azure since I typically have nothing running in either cloud.  It showed me the monthly cost for each VM in both enviornments as an aggigate!  Now I know what my cloud spend is if I decide to use more than one cloud provider for my personal use.

![Mist Landing Page](/images/Mist_FrontPage.gif)

![Mist VM List](/images/Mist_VM_List.png)

From a VM management stand point you can connect to a shell, start/stop/delete VMs, run scripts you have imported from Git into Mist.  In public cloud you can create and attach block storage to your instances.  If the provider has DNS capabilties you can manage DNS from Mist as well.  On Digital Ocean and other providers it pulls the image list from their marketplaces and for KVM just provide Mist a path to your golden image on the KVM Host's file system.

![Mist VM Provision](/images/Mist_VM_Provision.png)

![Mist VM View](/images/Mist_VM_View.png)

Automation seems to be very flexible. For scripts, I just used Ansible playbooks.  You can put them right into Mist by coping and pasting the code or import them from Github.  I didn't try the direct executable script function but it looks like any scripting language should work as long as the VM can execute it.  I will explore that for  

A feature that I didn't get into deep was Rules.  This feature looks like a policy and alerting engine using a simple 'if/then' concept.  It can pull from logs or metics and take action on based on keywords in a log or a metric threshold.  This is very useful if you do not have complex monitoring already configured or just want to sublement existing monitoring capabilties.  It can also be used to run a script based on the rule or call out to other systems with webhooks.

The amount of features that Mist has it is the lowest barrier to entry for Cloud Management period.  If you don't need a complex management structure and you use Ansible today for your configuraiton management and automation Mist is a no brainer.  If you need something more complex with more fine-grained controls, can control more functions and is far more extensable, you need to take a look at ManageIQ.

Watch out for a how to video on how to get started with Mist in the coming days.  I will also be posting a detailed deep dive into ManageIQ along with some howto videos.