---
layout: post
title: AD Shadow Groups with Windows PowerShell An Update
date: 2012-10-24 21:12
author: Dave Green
comments: true
categories: [Computers]
---
I've made a fair amount of changes to the shadow group sync script, it's a fair amount more powerful and much more flexible than the first version. You can now sync user and computer objects into groups from an OU, but with the added functionality of filters to narrow for specific user or computer attribute values. There are a few changes to the format of the CSV, detailed in the script comments, along with an example of the filters that you can use (see the Microsoft [TechNet article](http://technet.microsoft.com/en-us/library/hh531527) on PowerShell Active Directory filter syntax for more information).

## UPDATE

There's some more changes here, the script now takes the CSV as an argument to the script file. For example:

    .\shadowGroupSync.ps1 ".\TheCSVFile.csv"
    .\shadowGroupSync.ps1 -file ".\TheCSVFile.csv"

are both valid ways or running the script.
As before, feel free to use it in any way you want. It's pretty straightforward, but may contain bugs which I take no responsibility for.

Current version:

- [See the GitHub Page](https://github.com/davegreen/shadowGroupSync.git)

Older versions:

- [shadowGroupSync-6](../assets/computers/shadowGroupSync-6.zip) (22/01/2013)
- [shadowGroupSync-5](../assets/computers/shadowGroupSync-5.zip) (20/01/2013)
- [shadowGroupSync-4](../assets/computers/shadowGroupSync-4.zip) (28/10/2012)
- [shadowGroupSync-3](../assets/computers/shadowGroupSync-3.zip) (24/10/2012)
- [i3laze-shadowGroupSync](../assets/computers/i3laze-shadowGroupSync.zip)
- [shadowGroupSync](../assets/computers/shadowGroupSync.zip)

Thanks to i3laze for his work with this. I probably wouldn't have made the effort to extend the script without it.