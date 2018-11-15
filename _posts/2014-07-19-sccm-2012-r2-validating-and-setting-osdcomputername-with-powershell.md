---
layout: post
title: SCCM 2012 R2 - Validating and setting OSDComputerName with PowerShell
date: 2014-07-19 14:52
author: Dave Green
comments: true
categories: [Computers]
---
I've recently been doing some work with System Center Configuration Manager (SCCM) 2012 R2 recently and I was interested in validating a computer name supplied during an Operating System Deployment (OSD) task sequence before actually attempting to set it and possibly causing an error (with the computer name being too long, for example). Since that sounds like a job for PowerShell, I immediately had a look and found a reasonable solution that *almost* fit my needs.

As an aside to this, I'm not currently using the [Microsoft Deployment Toolkit](http://technet.microsoft.com/en-gb/windows/dn475741.aspx) (MDT) in any real way, however my solution does use a component of it in order to display the PowerShell script to the user. This gave me a good excuse to run through it and start to check it out with the idea of using it more down the line.

The solution I found that almost did what I wanted can be found [here](http://www.scconfigmgr.com/2013/10/02/prompt-for-computer-name-during-osd-with-powershell) (I've contributed my alterations there too in the comments. Thanks very much to Nickolaj for his script, as it saved me a fair bit of work!

To run the script, a few things have to be done first (Instructions with screenshots can be found in Nickolaj's post on scconfigmgr.com.

- Add the 'WinPE-NetFx' and 'WinPE-Powershell' features to the boot image you will be using with the OSD. (in "Boot Images > Boot image > Properties > Optional components")
- Download a copy of [MDT](http://technet.microsoft.com/en-gb/windows/dn475741.aspx) that matches the boot image architecture you want (x86/x64), then extract the ServiceUI.exe file from it, usually located at "%ProgramFiles%\Microsoft Deployment Toolkit\Templates\Distribution\Tools"
- Create an SCCM package containing the script, plus ServiceUI, but don't create a program for it, as we'll deal with that bit when adding it to the task sequence.

Then, you add a 'Run Command Line' task in your task sequence, then use the package created  in the previous step, along with a command line like:

    ServiceUI.exe -process:TSProgressUI.exe %SYSTEMROOT%\System32\WindowsPowerShell\v1.0\powershell.exe -NoProfile -WindowStyle Hidden -ExecutionPolicy Bypass -File MYSCRIPTFILENAME.ps1

After testing the script and running it, there were a couple of things I saw that could be improved:

- First was the ability to hit enter and have that correspond to the OK button. It's  minor thing, but makes a massive difference to the user experience!
- Next, was the validation/correction of the computername. The original script will silently strip out invalid characters, which may leave some wondering why the computer name appears differently to how it was originally typed.

I started with making the enter button correspond to the OK button. I did this by adding the following code just before the call to load the form:

    $Form.KeyPreview = $True
    $Form.Add_KeyDown({if ($_.KeyCode -eq "Enter"){Set-OSDComputerName}})

Next, I wanted to remove the silent removal of user input and make it obvious to the user that they had entered an invalid computer name. I did this by re-using the existing ErrorProvider and moving the validation code to another clause in the if statement.

This meant that instead of having:

    else {
        $OSDComputerName = $TBComputerName.Text.Replace("[","").Replace("]","").Replace(":","").Replace(";","").Replace("|","").Replace("=","").Replace("+","").Replace("*","").Replace("?","").Replace("<","").Replace(">","").Replace("/","").Replace("\","").Replace(",","")
        $TSEnv = New-Object -COMObject Microsoft.SMS.TSEnvironment
        $TSEnv.Value("OSDComputerName") = "$($OSDComputerName)"
        $Form.Close()
    }

We end up with something slightly different (ignoring the use of a Regular Expression to validate the computer name instead of a multiple string.Replace())

    #Validation Rule for computer names.
    elseif ($TBComputerName.Text -match "^[-_]|[^a-zA-Z0-9-_]") {
        $ErrorProvider.SetError($GBComputerName, "Computer name invalid, please correct the computer name.")
    }
    else {
      $OSDComputerName = $TBComputerName.Text.ToUpper()
      $TSEnv = New-Object -COMObject Microsoft.SMS.TSEnvironment
      $TSEnv.Value("OSDComputerName") = "$($OSDComputerName)"
      $Form.Close()
    }

As you can see, this makes the script a little easier to read, which always bodes well for improvements in the future. One thing that still slightly annoys me is the format of the if elseif elseif else. This is quite close to being made a switch statement, but it's OK until I find the need to add another clause. I'd also like to find a way to remove the MDT dependency of ServiceUI.exe, as this requires a different SCCM package or invocation based on architecture (x86/x64). However, this may not be possible due to the way the OSD task sequence works.

[Here](https://gist.github.com/davegreen/453cee5ef2db1063a007) is the current script I'm using in full. Please let me know if you have any improvements you can suggest, as it's always a good day to learn!

[OSDComputerName](https://gist.github.com/davegreen/453cee5ef2db1063a007)