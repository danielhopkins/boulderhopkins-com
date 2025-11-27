---
title: Rapid development in Alfresco Share - the refresh webscript
date: 2010-11-01T19:26:00.001000
lastmod: 2010-11-02T14:40:44.467000
author: Dan
draft: false
tags:
  - Alfresco
  - Dev Environment
aliases:
  - /2010/11/rapid-development-in-alfresco-share.html
---

[Zia Consulting](http://www.ziaconsulting.com) develops in Eclipse using a setup different than described by Alfresco.  We run an embedded version of Jetty with the Alfresco default WARs.  At runtime we mix in changes using extra classpath entries and web overlays.  This has many benefits: most importantly that we develop functionality without having to restart the repository.  When there is a reason to restart the server we make efforts to fix the problem through programming or configuration.

Share has caches that make the webscripts and surf run faster.  These caches have documentation on how to turn them off but this documentation changes with every release of share or surf.  This is difficult to keep up with.

If you're interested in the documented process I have a list of articles to read:

[Old Surf intro](http://www.benh.co.uk/alfresco/surf-part-1-getting-started/) - refreshing the Surf page cache.  I don't believe this is needed in the most recent versions of share.

[Official wiki documentation](http://wiki.alfresco.com/wiki/Surf_Platform_-_Deployment_Configurations#Web_Framework_Configuration_2) - For Alfresco 3.0 ➔ 3.3, before Surf joined the Spring project.

[Official developer guide](http://wiki.alfresco.com/wiki/Surf_Platform_-_Developers_Guide) -  The section for debugging didn’t get finished.  Likely because they moved over to Spring.

I’m sure there are more.  Feel free to add them to the comments.

We have given up on following the documentation and developed a custom Web Script that invalidates these caches whenever a source file is changed.  The code for this Web Script is from the service index refresh that is part of “org.springframework.extensions.webscripts.bean.IndexUpdate”.  We paid particular attention to the reset method of “org.springframework.extensions.webscripts.AbstractRuntimeContainer”.  The reset method gets run when you goto http://localhost/share/service/index and click the reset button.  The important code is trivial:

```
  /* public void reset(){ ...*/  
  scriptProcessorRegistry.reset();  
  templateProcessorRegistry.reset();  
  getRegistry().reset();  
  configService.reset();  
  /* } */
```

Running all four of these resets takes a couple seconds and is impractical to do after every save.  Taking just the script and template reset and putting them in a Web Script keeps it pretty lean.  Running time drops from a 2 - 3 seconds to a few hundred ms.  It is fast enough that the refresh will happen before you can cmd + tab to your web browser and refresh.

We created a refresh Web Script using the above code and then setup an [Eclipse builder](http://www.eclipse.org/articles/Article-Builders/builders.html) to run when the source is updated.  To do this you need to create a few things.

- Refresh Web Script.  [It’s best to use “none” authentication.](http://wiki.alfresco.com/wiki/Web_Scripts#Creating_a_Description_Document)
- Ant script that will call the new Web Script.  Shown below.
- In Eclipse turn the directory containing javascript and freemarker templates (the config directory if your projects look like exploded amp files) into a source directory so it triggers the auto build.
- Create a builder according to [this tutorial](http://help.eclipse.org/galileo/index.jsp?topic=/org.eclipse.platform.doc.user/gettingStarted/qs-93_project_builder.htm) with one change:   
  - In step #11 they recommend *NOT*creating an auto build.  **Ignore this direction -** auto building is really handy for our purposes.  It doesn’t cause performance issues, so turn it on and configure it to run “run-refresh-webscript”.

The ant file that we use:

```
xml version="1.0" encoding="UTF-8"?  
  
    
          dest="refresh-results.dat" />
```

**Update:**

I found [this article](http://blogs.alfresco.com/wp/kevinr/2010/04/07/developer-tips-for-alfresco-share-33/) by Kevin Roast that has some great tips on Share development.  The “mode” selection is of great interest.