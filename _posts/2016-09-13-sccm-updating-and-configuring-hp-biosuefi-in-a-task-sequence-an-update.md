---
layout: post
title: SCCM – Updating and configuring HP BIOS/UEFI in a task sequence - An update
date: 2016-09-13 23:20
author: Dave Green
comments: true
categories: [Computers]
---
Lately I've bee working on a little more SCCM operating system deployment work and I've got an updated toolset for configuring and updating UEFI firmware for HP machines easily in a task sequence. This is a reasonably long post, so bear with me.

A lot of the same techniques [from my earlier posts](http://tookitaway.co.uk/tag/hp-bios-configuration-utility/) on the subject apply. We are still using HPBIOSUPDREC, BiosConfigUtility and an SCCM package for the source files. I've updated the batch files to take an argument for the configuration or update file, as well as the previous architecture detection. The good thing about this method is that it supports all current HP laptop, desktop and workstation models with no change, you just give the update file, or the configuration as an argument and away you go.

I considered using PowerShell for this, however it takes a little while to start in WinPE and unless I add more logic to the process for particular models or action types, I don't see the need to convert it yet.

I've set up the package for HP machines I need to configure and update as follows:

<pre>SCCMPackages\HP-UEFI\
    68ICF.CAB (UEFI firmware - EliteBook 8x70p)
    BiosConfigUtility.exe   (BIOS config utility - x86)
    BiosConfigUtility64.exe (BIOS config utility - x64)
    BIOSPW.bin              (Encrypted BIOS password)
    ConfigureUEFI.cmd (UEFI config command file)
    EliteDesk800G2-Win7.cfg (UEFI configuration - Win 7)
    EliteDesk800G2-Win10.cfg (UEFI configuration - Win 10)
    EliteBook8x0G3-Win7.cfg (UEFI configuration - Win 7)
    EliteBook8x0G3-Win10.cfg (UEFI configuration - Win 10)
    EliteBook8x70p-Win7 (UEFI configuration - Win7)
    N75_0110.bin (UEFI firmware - EliteBook 8x0 G3)
    N21_0219.bin (UEFI firmware - EliteDesk 800 G2 SFF)
    UpdateBIOS.cmd (UEFI update command file HPqflash models)
    UpdateUEFI.cmd (UEFI update command file HPBIOSUPDREC models)</pre>

A sample set of files for all of this can be [found on GitHub](https://github.com/davegreen/miscellaneous/tree/master/SCCMPackages/HP-UEFI), except the HP binaries and firmware, which need to be downloaded from HP.

You can follow the larger package format, with all models together, or spread the update and configuration files over multiple packages, whichever suits your requirements best.

You may notice that I've included an odd DLL file 'oledlg.dll'. This is needed to make HPqflash work on WinPE 10 (10.0.10586.0). If you run HPqflash in WinPE 10 without it, you get an exit code of -1073741515 (0xC0000135), which means a DLL needed for the program is missing.

I did a bit of investigation with procmon on a full windows system and found oledlg.dll was required, but missing from WinPE. I put this DLL in the same folder next to HPqflash and all was good!

Both ConfigureUEFI.cmd and UpdateUEFI.cmd are general for all models using HPBIOSUPDREC and the HP BIOS config utility and look like this:

https://gist.github.com/davegreen/ac642a9a5f4eab2b85a9e10bfbabe889

https://gist.github.com/davegreen/2caade16ace6f541b0789a084f6c7a86

There's also a slightly different version for deploying updated firmware, if you're still using HPqflash:

https://gist.github.com/davegreen/be569e1c4d10dd09772467e265c935f3

We can use the same SCCM 'Run command line' task we used in the past for this, with a little tweak to run the command file with the right update or configuration. This is done in the same way as before, with the extra exit code for successful completion.

This is the configure command line:

[<img class="alignnone size-full wp-image-1716" src="http://tookitaway.co.uk/wp-content/uploads/2016/09/Command-Line-Configure.png" alt="command-line-configure" width="466" height="246" />](http://tookitaway.co.uk/wp-content/uploads/2016/09/Command-Line-Configure.png)

Followed by the update command line, along with the success exit codes shown.

[<img class="alignnone size-large wp-image-1717" src="http://tookitaway.co.uk/wp-content/uploads/2016/09/Command-Line-Update.png" alt="command-line-update" width="466" height="244" />](http://tookitaway.co.uk/wp-content/uploads/2016/09/Command-Line-Update.png)

[<img class="alignnone size-large wp-image-1718" src="http://tookitaway.co.uk/wp-content/uploads/2016/09/Update-Exit-Codes.png" alt="update-exit-codes" width="468" height="135" />](http://tookitaway.co.uk/wp-content/uploads/2016/09/Update-Exit-Codes.png)

The update command line needs to follow the configure step if as it requires a password bin file.

Hopefully this has been helpful updating things in the journey to support newer HP models.
