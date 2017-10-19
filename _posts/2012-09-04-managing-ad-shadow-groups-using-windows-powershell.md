---
layout: post
title: Managing AD Shadow Groups using Windows PowerShell
date: 2012-09-04 21:44
author: Dave Green
comments: true
categories: [Computers]
---
I've done a bit of work with Windows PowerShell lately, here's a little script to sync AD OUs with 'Shadow Groups'.

I couldn't find a full solution that I could pick up and use, so I rolled my own to sync some OUs containing computers with some groups.

The script reads from a CSV that defines the source OUs and the group names you want to populate. Inside the script, the destination OU gets set, where the shadow groups will exist.

I've linked to the code below, feel free to use it in any way you want. It's pretty straightforward, but may contain bugs which I take no responsibility for.

Download: [shadowGroupSync.zip](http://tookitaway.co.uk/wp-content/uploads/2012/09/shadowGroupSync.zip)

## UPDATE

i3laze (i3laze 'at' yandex 'dot' ru) supplied an updated script that deals with syncing mail-enabled users and child domains, and not just computers. As with the first version, I take no responsibility if the script has bugs or somehow manages to nuke your domain.

Download: [i3laze-shadowGroupSync](http://tookitaway.co.uk/wp-content/uploads/2012/09/i3laze-shadowGroupSync.zip)

I've done some work to merge the two versions, which will give the script a lot more flexibility as to the object types it will sync, but there's still got a bit of testing to do before I post it.

## UPDATE 2

Newer versions of the script are [here](http://tookitaway.co.uk/ad-shadow-groups-with-windows-powershell-an-update/). This post is kept as a reference to the first version of the script.
