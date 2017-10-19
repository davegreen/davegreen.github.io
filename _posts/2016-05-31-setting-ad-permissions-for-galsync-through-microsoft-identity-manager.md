---
layout: post
title: Setting AD permissions for GALSync through Microsoft Identity Manager
date: 2016-05-31 23:08
author: Dave Green
comments: true
categories: [Computers]
---
A quick function for setting permissions in Active Directory to allow the Microsoft Identity Manager Global Address List (GAL) sync Management Agent to do its thing. I put it in a [GitHub gist](https://gist.github.com/davegreen/368303ff31ed7c0036d6e53debebccca) so it might help someone else!

It's also a good intro to look at for getting into dealing with Access Control Lists (ACLs) in AD, as it deals with setting [specific permissions](https://msdn.microsoft.com/en-us/library/ms679424.aspx) or [extended rights](https://msdn.microsoft.com/en-us/library/ms683985.aspx). Anyways, here's the script!

https://gist.github.com/davegreen/368303ff31ed7c0036d6e53debebccca
