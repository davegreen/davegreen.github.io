---
layout: post
title: Even more work with shadowGroupSync
date: 2013-01-22 23:04
author: dave
comments: true
categories: [Computers]
---
There has been a couple of updates to the shadowGroupSync tool over the past few days. Apart from a bit of development, I also I went and threw it to GitHub. You can check out my <a href="http://tookitaway.co.uk/tag/shadowgroupsync/">previous posts</a> on it, or grab the <a href="https://github.com/davegreen/shadowGroupSync.git">latest version</a>. <del>There's also a development branch in the repository that lets you choose the type of group to create.</del> UPDATE: The features from the development branch are now all in the stable version.

The stable branch has also had a few features added and a couple of little snags fixed. The main new feature is the ability to specify multiple source organisational units for inclusion into a single shadow group. These changes make the utility very useful for generating distribution lists or access groups based on whatever conditions you want; like employee type, manager, or existing group memberships.

The stable 'master' branch is essentially the script use myself on a day to day basis, so it should work fine for you. However, as with all things not done yet, the development branch 'next' may be unstable. If downloading it sets your computer/server/domain/datacenter on fire (delete as appropriate), that's got nothing to do with me.
