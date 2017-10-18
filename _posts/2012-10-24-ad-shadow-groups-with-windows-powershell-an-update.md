---
layout: post
title: AD Shadow Groups with Windows PowerShell An Update
date: 2012-10-24 21:12
author: dave
comments: true
categories: [Computers]
---
I've made a fair amount of changes to the shadow group sync script, it's a fair amount more powerful and much more flexible than the first version. You can now sync user and computer objects into groups from an OU, but with the added functionality of filters to narrow for specific user or computer attribute values. There are a few changes to the format of the CSV, detailed in the script comments, along with an example of the filters that you can use (see the Microsoft <a href="http://technet.microsoft.com/en-us/library/hh531527">TechNet article</a> on PowerShell Active Directory filter syntax for more information).
<h2>UPDATE:</h2>
<p style="padding-left: 30px;">There's some more changes here, the script now takes the CSV as an argument to the script file. For example:</p>

<blockquote>
<pre>.\shadowGroupSync.ps1 ".\TheCSVFile.csv"</pre>
<pre>.\shadowGroupSync.ps1 -file ".\TheCSVFile.csv"</pre>
</blockquote>
<p style="padding-left: 30px;">are both valid ways or running the script.</p>
As before, feel free to use it in any way you want. It's pretty straightforward, but may contain bugs which I take no responsibility for.

Current version:
<ul>
	<li> <a href="https://github.com/davegreen/shadowGroupSync.git">See the GitHub Page</a></li>
</ul>
Older versions:
<ul>
	<li><a href="http://tookitaway.co.uk/wp-content/uploads/2012/10/shadowGroupSync-6.zip">shadowGroupSync-6</a> (22/01/2013)</li>
	<li><a href="http://tookitaway.co.uk/wp-content/uploads/2012/10/shadowGroupSync-5.zip">shadowGroupSync-5</a> (20/01/2013)</li>
	<li><a href="http://tookitaway.co.uk/wp-content/uploads/2012/10/shadowGroupSync-4.zip">shadowGroupSync-4</a> (28/10/2012)</li>
	<li><a href="http://tookitaway.co.uk/wp-content/uploads/2012/10/shadowGroupSync-3.zip">shadowGroupSync-3</a> (24/10/2012)</li>
	<li><a href="http://tookitaway.co.uk/wp-content/uploads/2012/09/i3laze-shadowGroupSync.zip">i3laze-shadowGroupSync</a></li>
	<li><a href="http://tookitaway.co.uk/wp-content/uploads/2012/09/shadowGroupSync.zip">shadowGroupSync</a></li>
</ul>
Thanks to i3laze for his work with this. I probably wouldn't have made the effort to extend the script without it.
