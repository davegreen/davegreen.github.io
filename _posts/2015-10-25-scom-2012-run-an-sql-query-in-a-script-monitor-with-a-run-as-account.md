---
layout: post
title: SCOM 2012 - Run an SQL query in a script monitor with a Run as account
date: 2015-10-25 13:23
author: dave
comments: true
categories: [Computers]
---
I got asked a little while ago if SCOM could query an SQL database and generate an alert in certain circumstances. I was sure it could, since I've seen things do it before, but it wasn't something i'd done myself. It seems like a fairly basic thing to do, but all the documentation I found was a bit scattered across the web, so I'm noting it down here, with links to the sources I used!

Somehow I've managed to get a long way without doing any real SCOM Management Pack authoring... Until now! This made my day a nice learning curve,

Here's the summary of how I went about it.
<ul>
	<li>Create a new Management Pack (MP) to contain the changes.</li>
	<li>Create the run as profile in the MP.</li>
	<li>Create the monitor in the MP.</li>
	<li>Export the management pack.</li>
	<li>Add the run as reference to the monitor.</li>
	<li>Re-import the management pack.</li>
	<li>Complete the configuration, set up alerting and overrides, etc.</li>
</ul>
To make things easier we create the framework of the monitor and the run as profile first, to make the manual editing step easier.
<h1>Sources</h1>
Massive thanks to the following people and everyone posting about SCOM stuff, a lot of this stuff would be incredibly difficult without their help.
<ul>
	<li>Stefan Stranger - <a href="http://cid-3ac99c5995164f2b.skydrive.live.com/self.aspx/files/Steps-to-Associate-Run-As-Account-to-monitor.1.0.pdf">Associating Run As Accounts to monitors</a> (<a href="http://tookitaway.co.uk/wp-content/uploads/2015/10/Steps-to-Associate-Run-As-Account-to-monitor.1.0.pdf">Mirror</a>)</li>
	<li>Pete Zerger - <a href="http://www.systemcentercentral.com/wp-content/uploads/2009/04/HOW-TO_2-state_ScriptMonitor.pdf">How to create a 2 state script monitor</a> (<a href="http://tookitaway.co.uk/wp-content/uploads/2015/10/HOW-TO_2-state_ScriptMonitor.pdf">Mirror</a>)</li>
</ul>
<h1>Create a new Management Pack (MP) to contain the changes</h1>
I started by creating a management pack for whatever monitors I create using this method, since it will need editing. This should help to keep things tidy, allowing me to isolate the changes I'm making from the rest of the system, as is good practice.

Browse to Administration, then right-click on "Management Packs" and choose to create a new management pack.

<a href="http://tookitaway.co.uk/wp-content/uploads/2015/10/MPCreate.png"><img class="alignnone wp-image-1579 size-full" src="http://tookitaway.co.uk/wp-content/uploads/2015/10/MPCreate.png" alt="MPCreate" width="560" height="566" /></a>

I've named and described my management pack, obviously these will be different in your case.
<h1>Create the run as profile in the MP</h1>
Browse to the Administration tab, then right-click "Run As Configuration" and choose to "Create Run As Profile..."

<a href="http://tookitaway.co.uk/wp-content/uploads/2015/10/CreateRunAsProfile.png"><img class="alignnone size-full wp-image-1591" src="http://tookitaway.co.uk/wp-content/uploads/2015/10/CreateRunAsProfile.png" alt="CreateRunAsProfile" width="852" height="689" /></a>

Name and describe the profile how you need it, then choose to put it in the management pack we created. Add the run account you need, then create it.
<h1>Create the monitor in the MP</h1>
To create the monitor, I browsed to the Authoring tab, then to "Management Pack Objects &gt; Monitors" and filtered the scope to the following "SQL Server YYYY DB", depending on the SQL Server version containing the database you need to target. I'll be working with SQL Server 2014 for this example, but this method applies to all recent versions of SQL server.

<a href="http://tookitaway.co.uk/wp-content/uploads/2015/10/MonitorList.png"><img class="alignnone size-large wp-image-1582" src="http://tookitaway.co.uk/wp-content/uploads/2015/10/MonitorList-1024x546.png" alt="MonitorList" width="474" height="253" /></a>

Next we can go ahead and create a new Unit Monitor to hold our script we need to run against the DB. In this example I'll be using a timed script two state monitor.

<a href="http://tookitaway.co.uk/wp-content/uploads/2015/10/CreateScriptMonitor.png"><img class="alignnone size-full wp-image-1584" src="http://tookitaway.co.uk/wp-content/uploads/2015/10/CreateScriptMonitor.png" alt="CreateScriptMonitor" width="782" height="734" /></a>

The script monitor should be created in the new management pack we've just created, given a reasonable name and not enabled, since we will create overrides later for the databases this monitor will apply to.

<a href="http://tookitaway.co.uk/wp-content/uploads/2015/10/CreateScriptMonitor-2.png"><img class="alignnone size-full wp-image-1585" src="http://tookitaway.co.uk/wp-content/uploads/2015/10/CreateScriptMonitor-2.png" alt="CreateScriptMonitor-2" width="782" height="734" /></a>

For testing, I'll be setting the schedule of the script to 5 minutes (not shown) and I'll be using the following script as a test monitor, it's just a simple script to grab the last updated time of a collection and pass that back to the property bag for the alert if it's over 20 minutes old. Here's the script and just as importantly the parameters screen to show how the script has been set up.

Here's the script itself:

https://gist.github.com/davegreen/c4e4b2e7039475e058b4

Take note that the SQL query in the script is made up for this example, but the rest script can be used for many other things. I've used parameters for the database server and database name in the script, so the solution just relies on the script and return values being specified correctly in the lower half of the script.

<a href="http://tookitaway.co.uk/wp-content/uploads/2015/10/CreateScriptMonitor-4.png"><img class="alignnone size-full wp-image-1588" src="http://tookitaway.co.uk/wp-content/uploads/2015/10/CreateScriptMonitor-4.png" alt="CreateScriptMonitor-4" width="508" height="262" /></a>

Now we have the script and the parameters sorted out, we can turn our attention to the health expressions. In this example they are set up simply. If we get some records back, there's a problem. It's a simple way to go, and this way round we can hopefully extract some useful reason for why things are unhealthy.

<a href="http://tookitaway.co.uk/wp-content/uploads/2015/10/CreateScriptMonitor-5.png"><img class="alignnone size-full wp-image-1589" src="http://tookitaway.co.uk/wp-content/uploads/2015/10/CreateScriptMonitor-5.png" alt="CreateScriptMonitor-5" width="782" height="734" /></a>

<a href="http://tookitaway.co.uk/wp-content/uploads/2015/10/CreateScriptMonitor-6.png"><img class="alignnone size-full wp-image-1590" src="http://tookitaway.co.uk/wp-content/uploads/2015/10/CreateScriptMonitor-6.png" alt="CreateScriptMonitor-6" width="782" height="734" /></a>

We can grab the data from the property bag returned by the script and use the details in the health expression by grabbing the property by name as shown. Since this is a test monitor and isn't really critical, I'm going to leave the "Configure Health" step as default, and not configure an alert yet, then create the monitor.
<h1>Export the management pack</h1>
To export the management pack, browse back to the management pack we created at the beginning, right click it and choose to export it. This will give you an XML file we can go ahead and edit.
<h1>Add the run as reference to the monitor</h1>
I've attached the original and the run as versions of the management pack here, showing <em>almost</em> no difference between the two files! All I've done is bump up the version number and add the "SecureReference" ID from the secure references section to the chosen UnitMonitor. It's a little easier this way than adding the secure reference from scratch and makes the whole process a bit quicker.

<a href="https://gist.github.com/davegreen/9b4d6da5ef1a883bd5c4/revisions">Here's the diff between the two files on GitHub</a>.
<h1>Re-import the management pack</h1>
To re-import the management pack, import the MP back from disk. Make sure you don't change the filename, as this will cause issues with importing, as will any XML validation errors that may have been introduced.
<h1>Complete the configuration</h1>
Now we've got over all that, we can now apply an override and try out the monitor!

Navigate back to the Authoring tab and find the monitor you created earlier. Right click it, choose "Overrides &gt; Override the monitor &gt; For a specific object of class: SQL Server YYYY DB". Choose the database you want this monitor to apply to, then click OK.

<a href="http://tookitaway.co.uk/wp-content/uploads/2015/10/CreateOverride.png"><img class="alignnone size-full wp-image-1593" src="http://tookitaway.co.uk/wp-content/uploads/2015/10/CreateOverride.png" alt="CreateOverride" width="720" height="739" /></a>

Choose to override the "Enabled" parameter, then click OK.

You'll be able to see the management pack importing on the destination server, if you look at the event viewer for the destination server.

<a href="http://tookitaway.co.uk/wp-content/uploads/2015/10/EventMPRecieved.png"><img class="alignnone size-large wp-image-1595" src="http://tookitaway.co.uk/wp-content/uploads/2015/10/EventMPRecieved-1024x546.png" alt="EventMPRecieved" width="474" height="253" /></a>

Eventually, you should see the script run. It will log an event under the source "Health Service Script" and should hopefully post the data you need! In this example is a shot of the script failing and posting the error text specified in the script to the event log.

<img class="alignnone size-large wp-image-1596" src="http://tookitaway.co.uk/wp-content/uploads/2015/10/EventScriptError-1024x546.png" alt="EventScriptError" width="474" height="253" />

I remove the error state by updating the collection. A while later, the script runs again and sees that the error is resolved.

<a href="http://tookitaway.co.uk/wp-content/uploads/2015/10/EventScriptSuccess.png"><img class="alignnone size-large wp-image-1598" src="http://tookitaway.co.uk/wp-content/uploads/2015/10/EventScriptSuccess-1024x546.png" alt="EventScriptSuccess" width="474" height="253" /></a>

Now we've confirmed it's all working, we can set up the alert. Navigate back to the "Authoring" tab in SCOM and get to the "Alerting" tab in the properties of our monitor.

<a href="http://tookitaway.co.uk/wp-content/uploads/2015/10/CreateAlert.png"><img class="alignnone size-full wp-image-1597" src="http://tookitaway.co.uk/wp-content/uploads/2015/10/CreateAlert.png" alt="CreateAlert" width="560" height="612" /></a>

This is an example of a basic alert, including the collection last update time in the alert description by adding the property returned by the script.

I'll cover troubleshooting these kinds of scripts in another post soon, there's a few things to look at to make sure you've covered all the bases. The most important thing is returning the right variable types in the property bag.