---
layout: post
title: MacOSX (10.6) Managed Preferences without OSX Server
date: 2011-07-25 22:22
author: Dave Green
comments: true
categories: [Computers]
---
I have tested this in both a development and a production Windows Server 2008 R2 environment, extending the schema to allow Mac OSX computers to received managed preferences direct from the Active Directory (AD), and not requiring a separate OSX directory server to supply those preferences.

I don't understand why Apple don't just give people the pre-made ldif file, with it configured to add only missing classes and attributes. The current method involves having to buy OSX server to do this, which is crazy (This may have changed in 10.7, I don't know yet).

Anyway, below is the correctly configured ldif file that will add the required attributes and classes to an AD schema. The environment I tested it in was a standard AD, with schema extensions for System Center Configuration Manager (SSCM) applied.

##### The following file is supplied with absolutely no guarantees and may in fact cause your Active Directory environment to spontaneously combust, or develop an unhealthy craving for human brains

[ADSchemaExtension_OSX10.6.ldif](http://tookitaway.co.uk/wp-content/uploads/2011/07/ADSchemaExtension_OSX10.6.ldif_.txt)

You should add this schema modification to the AD schema, by using the following command:
    ldifde -i -u -f ADSchemaExtension_OSX10.6.ldif -s server:port -b username domain password -j . -c "cn=Configuration,dc=X" #configurationNamingContext

If you run this command on the schema master, you can omit the username/password and server/port part from the command, as so:
    ldifde -i -u -f ADSchemaExtension_OSX10.6.ldif -j . -c "cn=Configuration,dc=X" #configurationNamingContext

More information on ldifde is in the [technet article for ldifde](http://technet.microsoft.com/en-us/library/cc731033(WS.10).aspx).

Once the schema has been extended, you can treat the AD as a direct replacement for the OSX Open Directory Server in regards to managing preferences.

The next step is to join your Macs to the domain with the AD connector and set up your administration machine to control preferences for Macs in the domain, by installing the [Server Admin Tools](http://support.apple.com/downloads/Server_Admin_Tools_10_6) for OSX 10.6.
