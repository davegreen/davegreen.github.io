---
layout: post
title: More about MacOSX (10.6) and Active Directory
date: 2013-02-14 21:05
author: dave
comments: true
categories: [Computers]
---
After getting our Macs into Active Directory with nice managed preferences, I ran into a few problems, first of which was slow logons.

To fix it, I had to make a change on each machine to disable the Bonjour service advertisements. This made .local DNS lookups much faster and solved a lot of issues with users not being able to log on, or logons taking a very long time. This appears to contradict the behaviour described <a href="http://support.apple.com/kb/HT3473">here</a>, although it could just be my particular environment. The important bit says:
<blockquote>In Mac OS X v10.6, as long as your network's DNS server is properly configured, you do not have to make any changes on your client Mac. Host names that contain only one label in addition to local, for example "My-Computer.local", are resolved using Multicast DNS (Bonjour) by default. Host names that contain two or more labels in addition to local, for example "server.domain.local", are resolved using a DNS server by default. Additionally, Mac OS X v10.6 automatically detects when the local network operator has set up a name server that will answer name requests for a domain ending in ".local". It does this by checking to see if there is a Start Of Authority (SOA) record for the top level domain "local", which is how a DNS server indicates that it claims to have authority over a part of the DNS namespace. As long as the DNS server is properly configured with the required SOA record, Mac OS X v10.6 will detect this SOA record and automatically use this server to look up all host names in the domain.</blockquote>
From: Mac OS X v10.4, 10.5, 10.6: How to look up ".local" host names via both Bonjour and standard DNS - <a href="http://support.apple.com/kb/HT3473">http://support.apple.com/kb/HT3473</a>

The change I made is also on the <a href="http://support.apple.com/kb/HT3789">apple support knowlegebase</a>, but I have listed it below if it disappears from the apple site (as pages often seem to do). You will need to edit the following file as an administrative user. You should probably make a backup of this file first, as breaking it can apparently prevent your Mac from starting up.
<pre>/System/Library/LaunchDaemons/com.apple.mDNSResponder.plist</pre>
You will need to add the following line to the file, on the line before the declaration for the end of the array:
<pre>&lt;string&gt;-NoMulticastAdvertisements&lt;/string&gt;</pre>
This should make the file look as follows.
<pre>&lt;key&gt;ProgramArguments&lt;/key&gt;
&lt;array&gt;
    &lt;string&gt;/usr/sbin/mDNSResponder&lt;/string&gt;
    &lt;string&gt;-launchd&lt;/string&gt;
    &lt;string&gt;-NoMulticastAdvertisements&lt;/string&gt;
&lt;/array&gt;</pre>
Once you've saved the file, reboot the Mac. You can probably just reload mDNSResponder, but that's what I did.

The second issue was more my fault than anything else and relates to a permissions issue I was having with network storage.

If you're going to offer network attached storage to your Macs for home folders or shared storage, the user must have read permissions at the root of the share. The permission doesn't need to apply to sub-folders, so you can still manage permissions in a secure way; although it's a little strange as Windows XP and 7 machines don't seem to need it. I just chalked it up to an implementation difference in SMB, made the changes and left it at that.

I'll probably do a post on managed printing and printing to a Windows print server in the near future, but I've got to iron out a few more issues and get it all sorted out before I put it all together.
