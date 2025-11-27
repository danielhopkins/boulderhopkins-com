---
title: Connecting to an untrusted certificate in java
date: 2011-02-20T22:38:00.001000
lastmod: 2012-05-06T15:46:58.402000
author: Dan
draft: false
tags:
  - Alfresco
  - Java
aliases:
  - /2011/02/connecting-to-untrusted-certificate-in.html
---

I was getting errors(*unable to find valid certification path to requested target*) in [Alfresco](http://www.alfresco.com/) trying to connect to LDAP over ssl using an untrusted certificate and found a great hint on the interwebs that I thought I'd share.  
  
Most of the hits on google mention this post: <http://blogs.sun.com/gc/entry/unable_to_find_valid_certification>  
  
My only criticism of that article is that it only minimally addresses how to actually use the fixed keystore.  I downloaded their source and hacked it  up a little change the name of the certificate store from "jssecacerts" into "cacert" which is the default certificate store for all java programs.  My goal was to fix the certificate store for the entire machine.  
  
To install an untrusted certificate into your keystore the process is like this: