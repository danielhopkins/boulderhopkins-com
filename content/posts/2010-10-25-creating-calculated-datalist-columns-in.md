---
title: Creating calculated datalist columns in Alfresco Share 3.3
date: 2010-10-25T13:55:00.001000
lastmod: 2010-10-30T21:55:14.023000
author: Dan
draft: false
tags:
  - Alfresco
  - Data lists
  - Share
aliases:
  - /2010/10/creating-calculated-datalist-columns-in.html
---

Datalists out-of-the-box don't have a whole lot of functionality outside of simply capturing data.  A simple way to help users is to add calculated columns that transform other columns or related data from the repository.  In Alfresco 3.3 this functionality is straightforward and allows creating powerful datalists that even an Excel user could love.

For this demo we will create a part of a project management datalist that takes allows users to enter the following: Estimated to completion, estimated at completion, actuals and budget.  From these inputs we will generate the variance, percent complete and percent expended.

## Overview

- Create a node and type form filter
- Create a couple of new static fields
- Create a new context file to instantiate the filters
- Tell Share to display the fields in the forms

## Create a form filter

Form filters derive from an AbstractFilter and must implement the four abstract functions in that class.  Filters are called whenever the form service runs and will be pass either a node instance or a typedef.  I created this implementation when I didn't understand the difference so I implemented both.  I also liked that I could show zeros before the node was created.

```
  public class ProjectStatusFilter extends AbstractFilter﻿ {  
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
      ContentModelFormProcessor.generatePropertyField(  
        PmoModel.variancePropDef, form, varianceValue, AlfUtil  
        .services().getNamespaceService());  
      ContentModelFormProcessor.generatePropertyField(  
        PmoModel.percentCompleteDef, form, percentComplete, AlfUtil  
        .services().getNamespaceService());  
      ContentModelFormProcessor.generatePropertyField(  
        PmoModel.percentExpendedDef, form, percentExpended, AlfUtil  
        .services().getNamespaceService());  
    }  
  }  
  /* Left the other 3 fuctions for completeness */  
  @Override  
    public void afterPersist(Object item, FormData data, NodeRef persistedObject) {}  
  @Override  
    public void beforeGenerate(Object item, List fields,  
    List forcedFields, Form form, Map context) {}  
  @Override  
    public void beforePersist(Object item, FormData data) {}  
}
```

The key to this is the ContentModelProcessor.generatePropertyField.  In order for this to work we need to create a field to be passed in.  I created these fields as anonymous classes and put them into the model interface.

## Create new field definitions

I'll give one example, the others are simple variations.  This is the class that is built when you do any content modeling, and the documentation and possible values are the same.  [Check the alfresco wiki for more information](http://wiki.alfresco.com/wiki/Data_Dictionary_Guide).

```
static final PropertyDefinition percentExpendedDef = new PropertyDefinition() {  
    @Override  
    public boolean isStoredInIndex() {  
        return false;  
    }  
    @Override  
    public boolean isProtected() {  
        return false;  
    }  
    @Override  
    public boolean isOverride() {  
        return false;  
    }  
    @Override  
    public boolean isMultiValued() {  
        return false;  
    }  
    @Override  
    public boolean isMandatoryEnforced() {  
        return false;  
    }  
    @Override  
    public boolean isMandatory() {  
        return false;  
    }  
    @Override  
    public boolean isIndexedAtomically() {  
        return false;  
    }  
    @Override  
    public boolean isIndexed() {  
        return false;  
    }  
    @Override  
    public String getTitle() {  
        return "Percent Expended";  
    }  
    @Override  
    public QName getName() {  
        return PROP_CALC_PERCENT_EXPENDED;  
    }  
    @Override  
    public ModelDefinition getModel() {  
        /* This is the name of a model that this type will belong to,  
        it's at the top of a model file for example  
        ﻿  
        */  
        return AlfUtil.services().getDictionaryService().getModel(  
                MODEL_RISKS_DATA_LIST);  
    }  
    @Override  
    public IndexTokenisationMode getIndexTokenisationMode() {  
        return null;  
    }  
    @Override  
    public String getDescription() {  
        return null;  
    }  
    @Override  
    public String getDefaultValue() {  
        return null;  
    }  
    @Override  
    public DataTypeDefinition getDataType() {  
        return AlfUtil.services().getDictionaryService().getDataType(  
                DataTypeDefinition.INT);  
    }  
    @Override  
    public ClassDefinition getContainerClass() {  
        return null;  
    }  
    @Override  
    public List getConstraints() {  
        return null;  
    }  
};
```

## ﻿Create a new context file to instantiate the filters

In the usual places (e.g. WEB-INF/alfresco/extension/custom-form-context.xml) create a new context file and instantiate the filter.

```
  xml version='1.0' encoding='UTF-8'?  
        'http://www.springframework.org/dtd/spring-beans.dtd'>  
    
                class="com.ziaconsulting.forms.ProjectStatusFilter"   
          parent="baseFormFilter">  
            
        
                class="com.ziaconsulting.forms.ProjectStatusFilter"   
          parent="baseFormFilter">  
            
      
```

## ﻿Tell Share to display the fields in the forms

Using the standard method to modify a form in share using the form service tell the node and type forms to display.  This means creating a share-config-custom.xml and adding a few extra elements to your currently configured data list.

```
  ﻿xml version="1.0"?  
    
      
        
          
          
            
              
              
              
              
              
              
              
              
            
          
        
      
      
        
          
          
            
              
              
              
              
              
              
              
              
            
          
        
    
```

## Next Steps, upgrading to 3.4b

The upgrade to 3.4 isn't too bad.  Alfresco changed the API  and didn't bother to leave any hints about how to upgrade, but fortunately I was able to re-figure out how to do this.  I'll publish the updates later this week.