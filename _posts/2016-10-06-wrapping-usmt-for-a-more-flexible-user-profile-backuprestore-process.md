---
layout: post
title: Wrapping USMT for a more flexible user profile Backup/Restore process
date: 2016-10-06 21:34
author: dave
comments: true
categories: [Computers]
---
I've done a bit with the <a href="https://technet.microsoft.com/en-us/itpro/windows/deploy/usmt-overview">User State Migration Tool (USMT)</a> for both migrating and archiving user data. Instead of using the USMT integration process with SCCM, i needed something a bit more flexible that would potentially allow profile restores to multiple machines.

One of the scenarios I needed to support was user initiated backup and restore of the current user profile. To support this and meet the requirements, I needed the following things:

<ul>
    <li>Multiple profile backups must be permitted. If a user performs a profile backup and doesn't restore the data, the user must be able to backup their profile again without requiring outside intervention.</li>
    <li>Previous profile backups should be stored for a time, as users may forget to restore or files may need to be recovered from the USMT store.</li>
    <li>When a restore is done, the latest profile backup for that user should be restored. This allows previous backups to exist for IT to recover files from if required.</li>
    <li>Users don't necessarily have administrative rights, so the solution must run as the local system to backup the current user.</li>
</ul>

The current script is designed to run either as an SCCM application, or from a task sequence to get the most recent logged on user and backup that profile only, along with the SYSTEM profile (to catch files stored in the root C:, as an example). Restore is to work the same way and unpack the most recent profile backup onto the destination machine it is run on.

Backing up the current user profile when running as SYSTEM presented a bit of a challenge, as the script will need to be told to backup just the logged in user. In order to give this information to USMT, we need to investigate the <a href="https://msdn.microsoft.com/en-us/library/windows/desktop/hh830632(v=vs.85).aspx">Win32_UserProfile</a>  WMI class, to see who's logged on. It does this by being passed the domain SID of users that might log onto the machine, then filtering the instance of Win32_UserProfile to get the most recently logged on username. From there, you can store the user profile, or select the most recent profile backup for restore.

Here's two scripts I call from task sequences to give user initiated scan and load tasks. This gives the users the ability to self-service their own backups and restores with no intervention from IT.

https://gist.github.com/davegreen/54d688e91a002c4d0b8dac3a02fabc0d

&nbsp;
