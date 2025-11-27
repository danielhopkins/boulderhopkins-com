---
title: Remap Caps lock on a Macbook Pro
date: 2010-12-14T01:29:00.001000
lastmod: 2010-12-14T02:11:11.970000
author: Dan
draft: false
aliases:
  - /2010/12/remapping-capslock-on-macbook-pro.html
---

I was fortunate enough to receive a CR-48 from Google last week.  My impressions of it are more or less in line with what [MG Siegler over at TechCrunch](http://techcrunch.com/2010/12/12/cr-48-chrome-notebook-review/ "MG Siegler over at TechCrunch") has reported:  good battery, nice screen, light weight laptop.  The only problem is that I have needed to adjust my workflow when I'm working on my regular computer, a Macbook Pro to facilitate sharing documents between the two machines.  For example, I have migrated away from OmniOutliner to a comparable online version, [Workflowy](http://workflowy.com/ "Workflowy").  
  
The other major change is that my keyboard habits are changing.  With only a weekend usage of ChromeOS I already prefer having the caps lock key replaced with a button to open up new tabs in my browser.  
  
When I got into work this morning I immediately missed my new favorite key.  I decided to try and figure out a way to replace this missing behavior.  
  
A [recent article outlines](http://news.ycombinator.com/item?id=1995857 "recent article outlines") how to turn the caps lock key into a control, option or splat.  One benefit of an Apple approved method is that the keyboard caps lock light disables itself when you change the functionality.  A never ending flickering green light would have been a constant annoyance for me.

This approach isn't enough, I wanted more - to be able to open a new tab in Chrome.  I found the [PCKeyboardHack](http://pqrs.org/macosx/keyremap4macbook/extra.html "PCKeyboardHack") preference pane/kext.  Using the instructions provided I remapped the caps lock key to F14, or keycode 107 on the [slim apple keyboard](http://www.apple.com/keyboard/ "slim apple keyboard").

Now that it was remapped to a real keyboard code it wasn't difficult to change the new tab shortcut in chrome to [respond to F14](http://lifehacker.com/343328/create-a-keyboard-shortcut-for-any-menu-action-in-any-program "respond to F14").

These changes work on the laptop's keyboard in addition to the external keyboard and I'm very pleased with the results.