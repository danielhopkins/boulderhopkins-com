---
title: Thread safety with no threads in sight
date: 2012-05-06T15:16:00.001000
lastmod: 2012-05-06T15:32:47.076000
author: Dan
draft: false
aliases:
  - /2012/05/thread-safety-without-threads.html
---

When people discuss thread safety in code it is almost entirely within the context of threading in a single process. The problems that come up are races conditions, [data races](http://blog.regehr.org/archives/490), and unseen updates with solutions like locking, synchronized updates and immutability.  
  
This definition of thread safety is too focused. Much of the problems normally attributed to thread safety can be seen in programs that have no access to OS threads or are running on multiple servers.  
  
My primary work is in Ruby, where we normally labor under the illusion of thread safety by primarily deploying to a single threaded VM. The problem is that often this single thread hinders scaling. The standard solution is to take long running tasks and allow them to finish asynchronous in a queued process.  
  
The main web process and the backgrounded process don't share any data, so thread safety in the traditional sense isn't an issue, but if you step back to look at the application as a whole they \*do\* share a database. This database is often mutable and is therefore a big source of thread safety bugs. Data races are the most obvious, but lost updates and stale reads also need to be considered.  
  
How do you fix these problems? With the same concepts you would at the application layer but translated to whatever database you have.  
  
If you're using an ACID compliant RDBMS then you're probably used to transactions which correspond to locking and mutex constructs in most programming languages. You should also consider improving your schema so that the database can enforce as much of your business logic as possible. This way your database can reject data that would create an inconsistent state even if your application deems it okay (remember your application won't know about the other processes running simultaneously).  
  
To pick on Rails for a moment: most application built on Rails have their [database constraints in the application](http://stackoverflow.com/questions/2373262/are-foreign-keys-in-rails-generally-avoided). This creates tough scenarios like the following.  
  
     1. user = User.find\_by\_email("danielhopkins@gmail.com")  
  2. if !user  
  3.   User.create!(:email => "danielhopkins@gmail.com")  
  4. end  
     
An execution might look like this:  
    Process #1        |   Process #2  
                      |  
    ln 1 user = nil   |     
    ln 2 true         | ln 1 user = nil  
    ln 3 create!      | ln 2 true  
    ln 4 done.        | ln 3 create!  
                      | ln 4 done.  
      
A worst case scenario of process execution has created identical users. **Even with no threads involved we have a clear threading bug.**  
  
Of course there are ways to prevent this from happening in Rails, this is illustrating how threading issues can manifest in a single threaded application. The same thing would apply to Java or any of the functional languages that pride themselves on their threading capabilities.  
  
**Remember that thread safety doesn't always involve threads.**  
  
It's a broader application problem that requires diligent understanding of how your application interacts with shared state. None of us are immune; especially in the world of internet applications.