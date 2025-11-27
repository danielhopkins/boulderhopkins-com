---
title: Attaching to a running Alfresco instance - running code in Alfresco
date: 2010-10-18T21:54:00
lastmod: 2010-10-30T21:42:40.157000
author: Dan
draft: false
tags:
  - Alfresco
  - Java
aliases:
  - /2010/10/attaching-to-running-alfresco-instance.html
---

A coworker and I have been scheming for a number of months on ways to run migration scripts and develop workflow in Alfresco.  The common thread is that we have a repository that's running and we want run arbitrary code against it.  We have had a number of ideas that have taken us in two different directions.  I'd like to talk about a few of them, outline the issues we've run into and then outline our merged solution.

## Migration

One approach (the merits depend on the project) to migration is load a bunch of data into the repository and then run some code on the repository to translate the data, extract metadata or whatever.  This can be run with an external suite of tools (e.g. an ETL), but this is often overkill.  The code to accomplish the same thing in Alfresco is often easier than 3rd party tools, involves less tools or languages and can be cheaper.  There are a number of ways to run javascript or java code.

### Command Servlet

There is an old (from the age of the documentation) servlet that can run arbitrary javascript that seems to fit the bill except that it will only run code stored as nodes in the repository and only in "insecure" mode, which means no access to native java and you have to develop over cifs/webdav or directly in a wysiwig editor in Alfresco.  None if this is ideal.

[Command Servlet](http://wiki.alfresco.com/wiki/URL_Addressability#CommandServlet)

[Secure webscripts](http://forums.alfresco.com/en/viewtopic.php?f=3&t=20500)

### As a webscript

Creating a web script gives you access to the real Rhino engine, i.e. "secure mode" or could write the script in Java giving you access to the entire API.  This is kludgy because you're executing your migration script by refreshing a webpage.  Also, the "standard out" is either logging messages or the free marker template.  The last nail in the coffin is that you must run your script in a massive transaction.  This seems to really slow down the system and if you are processing more than 1000 nodes you will get cache error messages.  Turning off the caching in the web script's descriptor file means no more access to the company home.

## Workflow development

The slowest part of workflow development is stepping through a long workflow.  To speed up the development process my coworker has developed a series of junit tests that setup the workflow and run it to a particular step that he's working on.  This allows all of the previously developed steps to be skipped over so that he can develop and debug the current step, it also provides a certain level of integration/regression testing.  He also spends a lot of time developing the java parts of workflow from within a breakpoint, allowing the jvm to swap the class files and then stepping through the code.

There are couple issue with this approach.  The first is the startup and memory usage.  The junit test spins up an entire alfresco repository, which is slow and very memory intensive.  Since it is sharing the same alf\_data as the real repository it tends to corrupt the lucene indexes and sometimes the entire repository.  He ends up dropping and reloading the entire schema multiple times per day and reindexing the indexes.  Even with these drawbacks the development time improvements, due to faster iterations, are substantial.

## Java vm attachment - a new approach

Along with Java 6 came a new feature that I only recently discovered.  You can now [attach to a running virtual machine](http://download.oracle.com/javase/6/docs/technotes/guides/attach/index.html) and run code loaded from a jar file and pushed into the running virtual machine.  Conceptually I can write a program (using a Java main), bundle it up in a jar, load it onto a server with a running Alfresco repository and then execute the code which will run in the same virtual machine and therefore have access to the applicationContext and any managed beans.  It works like JMX, for developers.

If attaching to the VM is all you're interested in feel free to stop here and read through [this tutorial](http://blogs.sun.com/sundararajan/entry/using_mustang_s_attach_api) of the process.  If you're interest in a more Alfresco specific, or at least a more web container specific implementation then stay with me.

I took what I learned in that tutorial and created a program and agent main function.  Both are bundled into a jar file.  The jar has a manifest that makes it executable and takes a classpath directory that is added to the running container and a [Runnable](http://download.oracle.com/javase/1.4.2/docs/api/java/lang/Runnable.html) class to be run in a new thread from within the container.  The relevant code in the program class is:

```
String classPathToRunnable = args[0];  
String runnableClassname = args[1];  
String agentArgs = classPathToRunnable + "," + runnableClassname;  
List listAfter = null;  
try {  
  listAfter = VirtualMachine.list();  
  boolean connected = false;  
  for (VirtualMachineDescriptor vmd : listAfter) {  
    if (vmd.displayName().contains("Main")) {  
      vm = VirtualMachine.attach(vmd);  
      vm.loadAgent(agentJarPath, agentArgs);  
      connected = true;  
    }  
  }  
  if (!connected) {  
    System.err.println("Couldn't connect, never found the server.");  
  }  
} catch (Exception e) {  
  e.printStackTrace();  
}
```

ClassPathToRunnable is a directory, e.g. "bin", when run in a typical java environment.  The runnable classname is a fully qualified classname, such as "com.ziaconsulting.MyRunnable".  The classpath directory is going to be added to the classpath of the container and then we'll use reflection to call the class that is passed in.  The way the attach process works is to run on a jar file that lists a class to run in the manifest file.  I have a simple ant file that manages the build and the manifest file:

```
                  value="com.ziaconsulting.Attach" />  
                  value="com.ziaconsulting.Agent" />
```

Pay special attention to the "Agent-Class" manifest attributes.

The next step is to work with the agent main class.  This proved to be a very tricky part because of the complexity inherit with java containers, specifically the number of classloaders.  I developed code specific to Jetty's container which will probably not work with tomcat.  Though the ideas should be the same:

```
public static void agentmain(String agentArgs, Instrumentation inst) {  
 // Execute the class that was passed in  
 try {  
   // Split the arguments - should be a classpath and a class  
   String[] args = agentArgs.split(",");  
   if (args.length != 2) {  
     throw new IllegalArgumentException(  
     "Not enought arguments passed");  
   }  
   
   // Begin the jetty specific stuff, we are trying to get the  
   // classloader from the Alfresco web app  
   Server s = Main.server;  
   WebAppDeployer wap = s.getBeans(WebAppDeployer.class).get(0);  
   
   HandlerCollection h = wap.getContexts();  
   Handler[] handlers = h.getHandlers();  
   for (Handler handler : handlers) {  
     if (handler.toString().contains("alfresco")) {  
       if (handler instanceof WebAppContext) {  
         WebAppContext wac = (WebAppContext) handler;  
   
         WebAppClassLoader cl = (WebAppClassLoader) wac  
         .getClassLoader();  
   
         // Check and see if this classpath is already added, if  
         // not then add it  
         boolean classPathAlreadyExists = false;  
         for (String entry : System.getProperty(  
           "java.class.path").split(  
           System.getProperty("path.separator"))) {  
             if (entry.equals(args[0])) {  
             classPathAlreadyExists = true;  
             break;  
           }  
         }if (!classPathAlreadyExists) {  
           cl.addClassPath(args[0]);  
         }  
         // Run the passed in class  
         final Class runnableClass = wac.getClassLoader()  
         .loadClass(args[1]);  
         final Thread thread = new Thread(  
         (Runnable) runnableClass.newInstance());  
         thread.setContextClassLoader(cl);  
         thread.start();  
   
       }  
     }  
   }  
 } catch (Exception e) {  
   e.printStackTrace();  
 }
```

This may seem like beating around the bush a little, my goal was for all the agent/attaching code to be abstracted so that all users have to do is create their own runnable class that knows nothing about how to get into the VM.  This is demonstrated in the simplicity of my runnable class:

```
public class MyRunnable implements Runnable {  
  public void run() {  
    try {  
      ApplicationContext ac = AlfUtil.applicationContext;  
      System.out.println(ac.toString());  
    } catch (Exception e) {  
      e.printStackTrace();  
    }  
  }  
}
```

There isn't much more to show.  I haven't had time to expand this to anything more than a proof of concept, but even at this stage it's pretty compelling.

The output of this code is directed to the stdout of the container e.g. catalina.out or alfresco.log.