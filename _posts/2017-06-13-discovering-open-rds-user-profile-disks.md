---
layout: post
title: Discovering Open RDS User Profile Disks
date: 2017-06-13 11:38
author: dave
comments: true
categories: [Computers]
---
Another week, another quick script!

Very occasionally, we have a problem with Remote Desktop User Profile Disks (UPDs) getting stuck mounted when there's a problem with a RDS session host and it either reboots or crashes. Here's a little script I've written to show the open VHDX files on a file server or SOFS cluster to find out which RDS host the VHDX is mounted on. Because user profile disks are quite fragile, there is a lot of odd behaviour around when a host crashes. When users log in again, the profile disk may mount on the new host, but the user always seems to receive a temporary profile and the disk gets stuck even after the user is logged out.

After this, you can then take action to unmount it and then deal with the disk. Anyways, here's the script!

