---
title: Using Jar deployment to solve conflicts with multiple customizations in Alfresco Share
date: 2010-09-02T21:33:00
lastmod: 2010-10-30T22:06:10.589000
author: Dan
draft: false
tags:
  - Alfresco
  - Share
aliases:
  - /2010/10/using-jar-deployment-to-solve-conflicts.html
---

![Screen shot 2010-09-02 at 10.09.53 AM.png](http://www.ziaconsulting.com/wp-content/uploads/2010/09/Screen-shot-2010-09-02-at-10.09.53-AM.png)  

Installing customizations using AMPs into a new Alfresco repository is pretty easy, there isn’t much room for configuration overwriting other configuration.  In older systems that have been installed by a client or in systems that have multiple SI’s customizations installed it maybe the case, especially in Share, where configuration from one amp overwrites the configuration of another amp.  You can get into difficult to detect problems depending on the order in which amps are applied to Alfresco (probably alphanumeric, last one wins).

  
With the release of 3.3 Alfresco has created a new way to deploy customizations to Share using Jar files and some trickery with Servlets to handle loading web resources e.g. Css, and client-side javascript.  The major benefit with this approach is that it allows developers to have some config files loaded from the jar file, where they won’t be overwritten.  Not all config, it seems that only the alfresco config i.e. One that end with -custom can be loaded this way.  You can see the list in the file slingshot-application-context.xml:  
  

  

```
 classpath:org/springframework/extensions/webscripts/spring-webscripts-config.xml
```

```
 classpath:META-INF/spring-webscripts-config-custom.xml
```

```
 jar:*!/META-INF/spring-webscripts-config-custom.xml 
```

The files that can be deployed this way in Share are:

- spring-webscripts-config-custom.xml
- spring-surf-config-custom.xml
- share-config-custom.xml
- spring-surf-config-custom.xml
- spring-webscripts-config-custom.xml

It turns out you can also do this in DM Explorer for web-client-config-custom.xml.

The idea here is not to stop using amps to deploy the code but instead to include the config *within the jar* with the other java code, instead of bundling it *within the amp*.

Amp structure before:

```
/amp
```

```
/lib
```

```
myAmp.jar ** Showing the contents
```

```
/com
```

```
/ziaconsulting
```

```
/MyClass.class
```

```
/config
```

```
share-config-custom.xml
```

```
   /web
```

```
And then after:
```

```
/amp
```

```
/lib
```

```
myAmp.jar ** Showing the contents
```

```
/META-INF
```

```
share-config-custom.xml
```

```
/com
```

```
/ziaconsulting
```

```
/MyClass.class
```

```
/config
```

```
/web
```

Using this idea it’s going to be very unlikely that another customization will override the share-config-custom.xml.

## Creating the jar

Using this information we can create a project to deploy to share and include a share-config-custom.xml without fear that another amp will overwrite our configuration e.g. Records Management.

If you use ant to build the amp file your jar task might look like this.

## Deploying the finished product

Assuming that your build is already creating an amp file the amp is deployed just like usual.  If you want to double check that the new source file is being picked up then turn on debug logging for the UrlConfigSource.  Open up the log4j.properties in the share webapp (typically tomcat/webapps/share/WEB-INF/classes/log4j.properties) and add this to bottom of the file.

Log4j.logger.org.springframework.extensions.config.source.UrlConfigSource=debug

If all has gone to plan you should see a log message from that class.

```
13:28:36,038  INFO  [config.source.UrlConfigSource] Found META-INF/share-config-custom.xml in
```

```
file:/Users/dhopkins/Public/work/test/alfresco-enterprise-tomcat-3.3.1/tomcat/webapps/share/WEB-INF/lib
```

```
/pmoShare.jar
```

```
 
```

That’s what we want to see.