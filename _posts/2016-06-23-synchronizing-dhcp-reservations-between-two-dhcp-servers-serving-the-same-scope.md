---
layout: post
title: Synchronizing DHCP reservations between two DHCP servers serving the same scope
date: 2016-06-23 19:44
author: Dave Green
comments: true
categories: [Computers]
---
As part of a DHCP server upgrade to Server 2012 R2, I needed to consolidate the reservations created between a single DHCP scope spread over two servers so I could have a definitive reservation list for the upgrade.

Here's the script I wrote to do it.

[Get-UnmatchedDhcpServerv4Reservation](https://gist.github.com/davegreen/f8390efe62c405ca8190d4a48bfeca0b)

You can use the function to add the DHCP reservations to the other DHCP server, or record the output with something like the following.

    Get-UnmatchedDhcpServerv4Reservation -ReferenceComputerName DHCP-Server-1 -DifferenceComputerName DHCP-Server-2 -ScopeId (((Get-DhcpServerv4Scope -ComputerName DHCP-Server-1 |
    Select-Object -ExpandProperty scopeId).ipAddressToString)) |
    ForEach-Object { Add-DhcpServerv4Reservation -ScopeId $_.ScopeId -ComputerName $_.MissingOnComputer -IPAddress $_.IPAddress -ClientId $_.ClientId -Description $_.Description -Name $_.Name -Type $_.Type -WhatIf }