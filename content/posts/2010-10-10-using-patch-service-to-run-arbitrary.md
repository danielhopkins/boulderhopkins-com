---
title: Using the patch service to run arbitrary code on AMP install
date: 2010-10-10T21:49:00
lastmod: 2010-10-30T21:39:00.621000
author: Dan
draft: false
tags:
  - Alfresco
aliases:
  - /2010/10/using-patch-service-to-run-arbitrary.html
---

There seems to be many times that the Alfresco repository needs to be modified when an AMP is first installed e.g. migration of current nodes.  While Alfresco provides a data bootstrap mechanism it appears it is inadequate for anything except the most basic tasks.  This post was predicated on finding a use of the patch service in the WCM installation script.

I haven’t seen many docs on the proper or improper use of the patch service.  At first blush it is used as a Repository internal tool to update the repository during major releases and hotfixes. As I’ve mentioned, the inspiration for this came from the WCM "installation" during which you insert the wcm-bootstrap-context.xml into the repositories classpath and then start it up.  In that file is a new bean that uses the patch service to create a couple of folders.

## Creating my own patch

To create a new patch we need a couple of things

- A context file to create the spring beans
- A properties file with description information
- A java class to instantiate that will run the patch

The context file can exist any where in the alfresco/extension package.  It should contain a bean that inherits from the basePatch abstract bean.

```
  parent="basePatch">  
    
    patch.pmoUsers  
    
    
    patch.pmoUsers.description  
    
    
    0  
    
    
    ${version.schema}  
    
    
    10000
```

﻿This is a simple bean that inherits from basePatch.  The settings that are the most important are the three versions that are passed in (﻿fixesFromSchema, fixesToSchema and targetSchema).  This is in contrast to the way that Alfresco typically uses this service, where the patch is meant to be applied to one specific schema.

The "id" is written to the patch table in the database and the description is a reference to a key in a properties file that has been loaded by the i18n Resource Bundle Bootstrap Component.

The properties file looks like any normal properties and is loaded by the I18N service.  The bean that we will use is:

```
class="org.alfresco.i18n.ResourceBundleBootstrapComponent">  
      
    

  
      alfresco.extension.pmoPatches
```

Nothing fancy here.  Just make sure that the ID is unique so you don't stomp the original message bundle bean.

The last thing to create is the java class that runs when the patch is installed.  It's pretty easy to find loads of examples in the repository of other patches and copy the ideas.  Here is one for completeness.

```
public class FreshProjectData extends AbstractPatch {  
...... La la la, details ......  
@Override  
protected String applyInternal() throws Exception {  
    // Create the new user group  
    String freshProjectAdmingroup = services.getAuthorityService()  
      .createAuthority(AuthorityType.GROUP, groupAuthorityName);  
    services.getAuthorityService().setAuthorityDisplayName(  
      freshProjectAdmingroup, groupDisplayName);  
    return "Created " + freshProjectAdmingroup;  
  }  
}
```

One thing to note is that the return string is placed into the "report" field in the database (see next section).

## Changes to the database

The database stores a record of the applied patch in the alf\_applied\_patch table.  Which you shouldn't need to check unless you're curious.

![Screen shot 2010-09-03 at 9.07.48 PM.png](http://www.ziaconsulting.com/wp-content/uploads/2010/10/Screen-shot-2010-09-03-at-9.07.48-PM1.png "Screen shot 2010-09-03 at 9.07.48 PM.png")