---
layout:	post
title:	Multi/Hybrid Cloud
date:	2021-03-15
categories: Cloud, Multi-Cloud, Hybrid-Cloud, Open-Hybrid-Cloud
---

I've gotten into agruements with my fellow technologist around what Multi or Hybrid Cloud means and if it is even needed.  I personally believe that the concept of Hybrid cloud is not only essential but the future of computing for many companies around the globe.  Multi-cloud and Hybrid-Cloud can mean the same thing in some reguards but here is my personal defination of both.

Multi-Cloud to me simply means you have multipule cloud enviornments, whether those be public or private, that are managed independently or centrally.  Hybrid-Cloud end goal is to get to the point where your applicaitons run where it is most economical at any given point with full data mobility.  Right now that is more or less a pipe dream unless you have the deep pockets to drop dedicated connections into each public cloud you are utilizing.  Right now my point of view of hybrid-cloud is abstracting the public cloud providers with robust container developer platforms and solid management.

I work with some of the largest Telecommunication providers and Service Providers in the world.  Many of these companies not only have their legacy on-premise infrastructure.  Many also a private cloud usually built on some flavor of OpenStack (either Red Hat or directly upstream) or VMware.  In many of these organizations IT still have not adopted public cloud, usually because of regulatory issues.  Even though their IT departments have not adopted public cloud, due to the needs of the business and IT not adapting quickly enough for their needs many business units in these organizations have deployed workloads on the public cloud and usually it isn't the same one and if it is the same one they have different accounts.  IT in these organizations have now been tasked to pull everything together and manage both public and private cloud.

There are two schools of thought here and I actually think that parts of both a applicable.  Centralized Management with a single pane of glass across legacy infrastructure, private and public and/or putting a layer between the end user (usually a developer) and the public cloud.  

The Centralized Management school of thought basically says to use the native capabilities of the cloud but have it be managed, regulated and monitored from a single tool that can handle the deployment of resources with approval flows.  This allows IT to control costs and decide which service they wish to expose to an end user.  These are tools that I have talked and written about in the past notiblity Mist.IO and ManageIQ.  Implementing these tools to not only get a view and gettng control of costs also provides self-service and automation which allows the business to still have the agility it is looking for when the business owner got out the credit card and started developing their new application on the cloud.

The second school of thought putting a layer between the developer and the underlying infrastructure.  This approach is usually implementing a Kubernetes platform like OpenShift/OKD that has it's own developer experience.  This apporach can also utilize native public cloud resources such as DBaaS and Object Storage as the developer needs it.  The platform between the developer and infrastructure provider gives a developer a consistant experience reguardless of what the underlying infrastructure is and it gives IT control over what services are being utilized and best places the workload based on where the data is living.  Even if public cloud isn't an option for an organization adding a container platform with a robust developer and operations experience on premise can add consistatancy across different onpremise environments whether that be proprietary virtualization or an open source private cloud platform.

I think both approaches have merrit.  I actually think every organization that has more than one cloud needs to manage it centrally with a cloud management tool, if not to do the provisioning but to at least have visibilty on what is running and have cost visibility.  Organization that are concerned with lock-in and have much of their data outside of the public cloud, utilizing a platform like OKD or Rancher can make the journey easier if public cloud adoption is on the roadmap and it gives a consistant developer framework regardless of public cloud provider.

Go and check out the two recent episodes of the Sudo Show where Eric and I have a discussion on multi-cloud and talk with Chris Psaltis.

[Multi-Hybrid Cloud What](https://sudo.show/17)
[Managing Multi-Cloud with Chris Psaltis](https://sudo.show/18)

Also don't forget to subscribe to the podcast and watch out for our new YouTube/Odysee channel in the coming months!
