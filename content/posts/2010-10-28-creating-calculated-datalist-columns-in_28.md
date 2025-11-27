---
title: Creating calculated datalist columns in Alfresco Share 3.4b
date: 2010-10-28T14:17:00.001000
lastmod: 2010-10-30T21:54:19.746000
author: Dan
draft: false
tags:
  - Alfresco
  - Data lists
  - Share
aliases:
  - /2010/10/creating-calculated-datalist-columns-in_28.html
---

This is a short one since not much changed.   If you didn't have a chance to read the 3.3 version of this [please review it now](http://www.boulderhopkins.com/2010/10/creating-calculated-datalist-columns-in.html) since I'm not going to post the entire tutorial again. We only need to change the filter and specifically how the dynamic columns are added to the form.

```
﻿public class ProjectStatusFilter extends AbstractFilter﻿ {  
  @Override public void afterGenerate(Object item, List fields,  
  List forcedFields, Form form, Map context) {  
    ﻿boolean typeDefCorrect = (item instanceof TypeDefinition && ((TypeDefinition) item)  
      .getName().equals(PmoModel.TYPE_PROJECT_STATUS_DL));  
    boolean nodeRefCorrect = (item instanceof NodeRef && AlfUtil.services()  
      .getNodeService().getType((NodeRef) item).equals(  
      PmoModel.TYPE_PROJECT_STATUS_DL));  
    ﻿if (typeDefCorrect || nodeRefCorrect) {  
      double varianceValue = 0d;  
      double percentComplete = 0d;  
      double percentExpended = 0d;  
      // If this is a node then do the calculations  
      if (nodeRefCorrect) {  
        NodeRef itemNode = (NodeRef) item;  
        /* Some calculations that aren't interesting */  
      }  
      /* Here are the changes for 3.4b */  
      form.addField(FieldUtils.makePropertyField(﻿PmoModel.variancePropDef,  
      varianceValue,﻿AlfUtil.services().getNamespaceService()));  
    }  
  }  
  /* Left the other 3 fuctions for completeness */  
  @Override  
  public void afterPersist(Object item, FormData data, NodeRef persistedObject)  
  {}  
  @Override  
  public void beforeGenerate(Object item, List fields,  
    List forcedFields, Form form, Map context) {}  
  @Override  
  public void beforePersist(Object item, FormData data) {}  
}
```