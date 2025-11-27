---
title: Creating custom JMX MBeans for reporting in Alfresco
date: 2010-11-08T15:10:00.001000
lastmod: 2010-11-08T15:10:00.544000
author: Dan
draft: false
tags:
  - Alfresco
  - Java
aliases:
  - /2010/11/creating-custom-jmx-mbeans-for.html
---

JMX rocks.  When configuring a server it is a boon to developers.  Especially when combined with the Alfresco [subsystem architecture](http://wiki.alfresco.com/wiki/Alfresco_Subsystems).  You can interate on changes to the LDAP sync without having to restart the server.  JMX also gives savvy system administrators a way to manage and monitor what’s going on within the repository.

If you’re still unfamiliar with the basics of JMX, especially within the context of Alfresco, [Jarred Ottley](http://jared.ottleys.net/) over at Alfresco has written a number of excellent tutorials.  I’ve added some additional articles and come up with the list below.

Some links:

- [JMX basics in Alfresco](http://wiki.alfresco.com/wiki/JMX)
- [Some JMX console implementation](http://wiki.alfresco.com/wiki/JMX#Connecting_through_JMX_Consoles_.2F_JSR-160) - I prefer [Java VisualVM](https://visualvm.dev.java.net/), which comes with the JDK
- [JMX from the commandline](http://jared.ottleys.net/alfresco/alfresco-jmx-from-the-command-line)
- [Tunnelling JMX - for use with Amazon EC2 or any firewalled computer](http://jared.ottleys.net/alfresco/tunneling-debug-and-jmx-for-alfresco).  [3.2 sp2 and newer should review this update](http://jared.ottleys.net/alfresco/updated-tunneling-debug-and-jmx-for-alfresco) .

With the basics out of the way it is often interesting to create your own MBean that can report custom statistics or expose custom methods.  This tutorial creates a new MBean that shows the number of Asynchronous jobs being run.  Alfresco exports beans using standard [Spring practices](http://static.springsource.org/spring/docs/2.0.x/reference/jmx.html).  This makes keeps everything well documented.  The list of things to create is small:

- Context file to register new MBean
- Annotated Java class

## Context File

```
xml version="1.0" encoding="UTF-8"?  
  
  
    
      class="org.springframework.jmx.export.MBeanExporter">  
      
      
        
          
        
      
    
      class="org.springframework.jmx.export.annotation.AnnotationJmxAttributeSource"/>  
    
      class="org.springframework.jmx.export.assembler.MetadataMBeanInfoAssembler">
```

## Java Class

```
  @ManagedResource  
  public class WhySlow {  
    @ManagedAttribute( description = "Asynchronous actions left to run" )  
    public long getAsyncActions() {  
      AsynchronousActionExecutionQueueImpl aaeq = ( AsynchronousActionExecutionQueueImpl ) AlfUtil.getSpringBean( "defaultAsynchronousActionExecutionQueue" );  
      long ret = -1;  
      try {  
        Class c = aaeq.getClass();  
        Field[] props = c.getDeclaredFields();  
        Field tpeField = c.getDeclaredField( "threadPoolExecutor" );  
        tpeField.setAccessible( true );  
        ThreadPoolExecutor tpe = ( ThreadPoolExecutor ) tpeField.get( aaeq );  
        ret = tpe.getTaskCount() - tpe.getCompletedTaskCount();  
      } catch( NoSuchFieldException nsfe ) {  
        nsfe.printStackTrace();  
      } catch( IllegalArgumentException e ) {  
        e.printStackTrace();  
      } catch( IllegalAccessException e ) {  
        e.printStackTrace();  
      }  
      return ret;  
    }  
  }
```

The [annotations](http://www.jarvana.com/jarvana/view/org/springframework/spring/1.2.9/spring-1.2.9-javadoc.jar!/org/springframework/jmx/export/metadata/ManagedAttribute.html) are important for documentation in the console.  There are some reflection shenanigans that allows access to private fields.  Your implementation will not need much of this code, except for the annotations.

When this code is deployed the WhySlow mbean will appear at the top level, next to Alfresco node.  This is controlled by the key of the map passed into the “beans” property(*Zia:name=WhySlow*) and is explained in the [Spring docs](http://static.springsource.org/spring/docs/2.0.x/reference/jmx.html#jmx-naming).

## Wrap up

A bean created under JMX tends to keep the separation of concerns better than many of the alternatives.  We have created “consoles” in webscripts, but it seems to be difficult to train the sys admins to go to multiple places for configuration.  Once the repository is started it is consistent to point users at JMX for all administration and reporting.