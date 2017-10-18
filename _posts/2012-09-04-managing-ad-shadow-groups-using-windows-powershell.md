---
layout: post
title: Managing AD Shadow Groups using Windows PowerShell
date: 2012-09-04 21:44
author: dave
comments: true
categories: [Computers]
---
I've done a bit of work with Windows PowerShell lately, here's a little script to sync AD OUs with 'Shadow Groups'.

I couldn't find a full solution that I could pick up and use, so I rolled my own to sync some OUs containing computers with some groups.

The script reads from a CSV that defines the source OUs and the group names you want to populate. Inside the script, the destination OU gets set, where the shadow groups will exist.

I've linked to the code below, feel free to use it in any way you want. It's pretty straightforward, but may contain bugs which I take no responsibility for.

Download: <a href="http://tookitaway.co.uk/wp-content/uploads/2012/09/shadowGroupSync.zip">shadowGroupSync.zip</a>
<h2>UPDATE:</h2>
<p style="padding-left: 30px;">i3laze (i3laze 'at' yandex 'dot' ru) supplied an updated script that deals with syncing mail-enabled users and child domains, and not just computers. As with the first version, I take no responsibility if the script has bugs or somehow manages to nuke your domain.</p>
<p style="padding-left: 30px;">Download: <a href="http://tookitaway.co.uk/wp-content/uploads/2012/09/i3laze-shadowGroupSync.zip">i3laze-shadowGroupSync</a></p>
<p style="padding-left: 30px;">I've done some work to merge the two versions, which will give the script a lot more flexibility as to the object types it will sync, but there's still got a bit of testing to do before I post it.</p>

<h2>UPDATE 2:</h2>
<p style="padding-left: 30px;">Newer versions of the script are <a title="AD Shadow Groups with Windows PowerShell: An Update" href="http://tookitaway.co.uk/ad-shadow-groups-with-windows-powershell-an-update/">here</a>. This post is kept as a reference to the first version of the script.</p>