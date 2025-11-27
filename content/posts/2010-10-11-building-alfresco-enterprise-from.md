---
title: Building Alfresco enterprise from source v3.4 beta
date: 2010-10-11T21:54:00
lastmod: 2010-10-30T21:43:13.789000
author: Dan
draft: false
tags:
  - Alfresco
  - Dev Environment
aliases:
  - /2010/10/building-alfresco-enterprise-from.html
---

Our development environment runs off an exploded a war file in an embedded version of jetty, all running in Eclipse.  We typically build the war from source pulled from Alfresco Enterprise SVN.  Knowing how to do this means we can checkout from any of the development branches and get a feeling for how different features are progressing without waiting for the official build to be distributed.  It also give me a warm fuzzy to know how the beast is built.

Up until 3.4b it was an easy process to run "ant incremental" within the enterpriseprojects directory and then unzip the wars that were produced.  The build was changed in 3.4 so that this process now creates a Circular Dependency and fails to run immediately.

I posted this as a regression bug to [JIRA](https://issues.alfresco.com/jira/browse/ALF-5115) and got a response today from Paul Holmes-Higgin to try *ant -Dbuild.script=enterpriseprojects/build.xml -f continuous.xml distribute*.  This still causes a failure unless you setup extra some folder structure to support building all of the distribution packages (see [this discussion](http://forums.alfresco.com/en/viewtopic.php?f=10&t=18822)) but at least you get the war file in the assemble directory, which is good enough for me.

One thing he didn't mention is that you need to add version numbers as arguments to ant so that some of the jars that are created have valid filenames.

I settled on this:

```
$ ﻿ant -Dbuild.script=enterpriseprojects/build.xml  
-Dversion.major=0 -Dversion.minor=0 -Dversion.revision=0  
-f continuous.xml distribute
```

Again, this build **will fail** unless you setup the folder structure described in the above discussion.

Check for war files:

```
$ ls -l build/assemble/web-server/webapps/  
total 239080  
-rw-r--r--  1 dhopkins  staff    97M Oct 13 07:52 alfresco.war  
-rw-r--r--  1 dhopkins  staff    20M Oct 13 07:52 share.war
```

Which leads eventually to...

![Screen shot 2010-10-11 at 10.34.21 AM.png](http://bolderdanh.files.wordpress.com/2010/10/screen-shot-2010-10-11-at-10-34-21-am.png "Screen shot 2010-10-11 at 10.34.21 AM.png")

How sweet it is!