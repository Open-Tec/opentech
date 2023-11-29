---
layout:    post
title:    Lenovo X1 Carbon 6th Generation Review
date:    2018-07-31
categories: hardware, Linux, laptop
---

I just got a Lenovo X1 Carbon 6th Generation to permanently replace my Dell XPS 15 as my go-to roadwarrior system.  I decided that I wanted something lighter and something that had better battery life.  I travel about 100+ days a year with some trips that take me across the United States and sometimes around the world.  I'm no stranger to the struggle of keeping my system battery charged while traveling.

My X1 has a Core i7 8650U, 16 Gigs of Ram and a 512 Gig hard drive with a 1080p screen.  I'm running Fedora 28 Workstation, with basically a stock Gnome Desktop with a handful of extensions.  I've had this system now for 5 weeks, and it has far exceeded my expectations.  As soon as I took it out of the box, the first thing I noticed it is super light.  Red Hat issued me the third generation X1 a few years ago, and the 6th Gen X1 is noticeably lighter and smaller.  Like all ThinkPads I have owned and used in the past the build quality is second to none.

Let's talk performance.  It is fast!  My XPS 15 is only a year old, and my X1 feels much faster in nearly every respect, and I'm running the same OS with the same desktop environment, extensions, and software.  I've had some issues with Gnome on the XPS 15 that I'm not having on the X1.  For example, when I press the logout or shutdown button in Gnome on the XPS 15 it takes 10-20 seconds for the dialog to come up on the X1 it comes up as soon as I click the button.

The battery life on the X1 is the best I've ever had on any laptop I've owned.  On my latest cross country trip from Salt Lake to Raleigh, as far as I'm concerned the battery barely moved considering I need to charge my XPS 15 as soon as I land.  My X1 was at full charge when the flight took off, and by the time the flight attendant came on the intercom to ask passengers to put their computers away, it was at 70% charge.  I was doing email, some light web browsing and I watched a movie during the 4-hour flight.  After I landed I worked on the cab ride over to my hotel, and I continued to work in my hotel room until nearly midnight, and I still had 20% left on my battery!  Over 12 hours of use and I still had at least an hour left of battery life.  I almost bought an T480 because of the larger and hot swapable battery.  I'm very happy that I didn't buy the larger and heavier T480, the battery life on the X1 is more than enough even though 20+ hours of battery life with the 3+6 cell batteries on the T480 is very tempting.  The tradeoff of having the fast charge and the light weight system more than makes up for it.

When it comes to Linux compatibility, it is pretty good.  As I said, I'm running Fedora 28, and everything works out of the box except for suspend.  The X1 is using a new method for suspend, and it looks like that won't be supported until kernel 4.18, and the current kernel in Fedora is 4.17.   There are a couple of methods around this issue.  The one I opted for was to add acpi.ec_no_wakeup=1 to my grub configuration.  The downside is wake on lid open doesn't work.  You need to press the power button to wake the system, but I personally feel that isn't a deal breaker.  The other method was to apply some patches, but it seemed too hackish for my taste, so I'm opting to wait for an offical fix.

#### Update 
Lenovo released a firmware update that specifically fixes suspend on Linux.
