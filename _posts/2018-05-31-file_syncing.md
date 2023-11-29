---
layout:	post
title:	The File Syncing Dilemma
date:	 2018-05-31
categories: File Sync, Open Source
---


I was an early adopter of file syncing software.  While I was at Novell, they were using a product that they had developed called iFolder.  iFolder was a file syncing solution that Novell released in 2001 and later open sourced in 2004.  When I started using iFolder at Novell, I set about building my own iFolder server at home.  At the scale I was using it at it was reliable and the client worked perfectly on my MacBook Pro and my Linux Desktop.  Sadly Novell stopped developing iFolder around 2010/2011.  I needed to find an alternative.  I had such a hard time finding a solution I kept using it until 2015.

I started looking for a hosted or "Cloud" solution that could effectively replace iFolder, but ultimately, I ended up self-hosting.
I had particular requirements for the replacement solution.  First and foremost, it had to work across different client operating systems.  I have Mac and Linux boxes as well as mobile devices.  I need to be able to get and/or sync files to those systems.  Second, it had to replicate the iFolder workflow.  In iFolder, you could select an existing folder or create a new folder anywhere on the file system and sync it.  

Here are some of the Cloud services and self-hosted solutions I tried.  I hope this will help people that are looking for a good cross-platform file sync solution.

## Hosted Cloud Solutions

Cloud solutions have their advantages and disadvantages.  A great advantage is you personally don't need to host anything.  The problem is you are giving up on privacy and flexibility.

### DropBox

I tried DropBox for about a week.  It didn't take me very long to see that it wouldn't meet my needs.  You need to stick with the DropBox paradigm.  With DropBox, the files need to be in the DropBox folder, which is fine but didn't meet my criteria.

### Google Drive

Google Drive is great if you use Google services.  I'm not a big fan of Google services, and it was quickly ruled out because it suffers the same problem as DropBox and there is no client for Linux.

### Box

I actually really like Box.  I like the mobile client, and I like the Mac client.  They also expose their services via WebDav, which is a bonus.  I seriously considered Box but it suffers the same limitations as DropBox and there is no client for Linux.

### OneDrive

OneDrive was very tempting.  I maintain an Office365 subscription for my wife and for that one off DOCX or XLSX file that just looks like garbage in LibreOffice.  There is obviously a Windows and Mac client; however, there is an open source Linux client that someone put together.  The way they built the client, it follows the iFolder paradigm and frankly it would have worked for me.  I opted not to keep using it only because I didn't want someone's personal development project to come to a halt suddenly

### SugarSync

I was really disappointed that I couldn't use SugarSync.  SugarSync has the same basic workflow as iFolder.  Not having a Linux client was the only deal breaker.

### Tresorit

Tresorit is very similar to SugarSync meaning it has that iFolder style workflow.  However, it is more privacy-focused like SpiderOak.  When I looked at their product, they didn't have a Linux client.  It seems like they now have a Linux client (it is currently in beta).  Now that I know that I will need to give it a try.

### SpiderOak

Since I brought up SpiderOak, I might as well give it a shout out.  I personally didn't like SpiderOak for file syncing.  However, due to its low cost, I use it as a backup target.  It is great for backups and has already saved my skin 1 or 2 times.

## Self-Hosted

### OwnCloud/NextCloud

OwnCloud was a bit of a struggle for me.  I really wanted to like OwnCloud.  The web UI is great, and the administration of it is a breeze.  However, I hate the client.  I used OwnCloud as my primary file syncing service for about 6 months before I gave up.  I forced the iFolder workflow on it which was a bad idea given at the time file syncing on OwnCloud was dodgy, to say the least.  In the end, it really didn't work for me since the default and preferred behavior is too similar to DropBox.  
I also feel that OwnCloud/NextCloud is trying to do everything.  It is trying to be a media repository/player, a note taking application, an office suite, contact server, calendar server, and a photo library.  It tries to do all the things and more.  I feel like the lack of focus is a problem for the project.  I really do feel they need to focus on core functionality but frankly I don't know what the core functionality of OwnCloud/NextCloud is since it can do so much.

### Seafile

Seafile is the solution I ended up landing on.  I've been using with great success since late 2015/early 2016.

SeaFile checks off all the boxes for my file syncing.  Sync individual folders, cross-platform, and an excellent web interface.  A bonus is the ability to configure WebDAV or use the new SeaDrive client for machines that don't have a lot of disk space.
I find it to be effortless to maintain and it does one thing and one thing only, File Sync and it is really good at it.  My only complaint is that uses MySQL/MariaDB for the database.  I would instead it use PostgreSQL purely out of preference.  

I'm going to write up a full review of Seafile soon and how I automated upgrades to my server.







