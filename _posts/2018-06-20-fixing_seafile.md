---
layout:	post
title:  Fixing Seafile
date:	2018-06-20
categories:  open source
---

I've been running SeaFile for sometime and I've had some issues with power outages and other hardware issues.  
I few months ago and again today I ran into an issue where my libraries would not display in the client or in the webui.  In both instances this occurred after an ungraceful shutdown of my SeaFile virtual machine.  I just wanted to document this easy fix for everyone.

If you see behavior like this usually it is because there is just a problem with the seahub cache in /tmp.  Just delete /tmp/seahub_cache and restart the seafile services.  So far this has solved 100% of the problems I've had with SeaFile to date which have been these two instances.


 
