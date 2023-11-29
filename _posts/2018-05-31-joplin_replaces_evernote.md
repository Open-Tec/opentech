---
layout:    post
title:    Escape from Evernote
date:    2018-05-31
categories: opensource alternatives
---

I've been using Evernote for years.  I've enjoyed using it, but when I moved back to Linux, the lack of a supported native client made me rethink my position on Evernote.  I started toying with OwnCloud + qownnotes last year, but it didn't stick, partly because I moved off OwnCloud.

I came across Joplin when it appeared on Fedora Magazine, the default home page on Firefox in Fedora.  I downloaded it and gave it a shot.  It has all the features that I use in Evernote (Notes, Tasks, and Tags).  Joplin uses Markdown for the notes syntax, and it just creates plain text files on your file system for each Notebook, Note, Task, and Tag.  Six months in so far, Joplin is one of my favorite note-taking applications.

Joplin is written in NodeJS (yes it is electron) and is packaged as an AppImage for Linux.  It also has a ncurses style client that I enjoy using.  There are also apps for iOS and Android.  Joplin allows you to sync using Onedrive, Webdav, and Nextcloud.  I'm syncing my notes across 7 different systems, three laptops, a desktop, a digitalocean droplet, and 2 iOS devices using the WebDAV feature in Seafile.  So far syncing has been rock solid.

If you have been a long time user of Evernote migrating to another solution might seem daunting.  Joplin has a feature that can import your notes from Evernote as well as individual plain text markdown formatted files.  I immediately imported all my notes from Evernote and some plain text files that I had.  All my notes from Evernote went in perfectly as far as I can tell.

One thing that still is keeping me on Evernote was a Firefox extension for Evernote called Clipper.  Clipper allows you to take screenshots, pull individual text, or save an entire web page in Evernote.  Just today I discovered there is a new feature in Joplin that enables the use of a FireFox extension with similar features called Joplin Web Clipper.  I tested it on a few websites, and the output was much better than I expected.  I hope to see more improvements on this over the next few months.

Right now I only have one complaint, Joplin stores every file on your file system and names the file with a hash.  If I'm on a system that doesn't have Joplin installed, it isn't evident what is what based on the file name.  I'm not too concerned about it unless I migrate from Joplin to something else down the line.

Joplin for me isn't just an Evernote replacement; it is an Evernote killer. 


UPDATE: I backed the Joplin project on their Patreon.  I decided to give them what I was paying Evernote for my premium subscription.  If you use it, I suggest you do the same to keep the development going.  [Joplin's Patreon](https://www.patreon.com/joplin).  Please do the same with other Open Source software you use.
