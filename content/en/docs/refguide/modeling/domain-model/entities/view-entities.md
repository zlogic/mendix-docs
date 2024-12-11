---
title: "View Entities"
url: /refguide/view-entities/
weight: 25
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## 1 Introduction

A view entity represents the result set of a stored [OQL query](/refguide/oql) and can be used similarly to a persistable entity. This concept parallels the function of views in general database technology. Whenever a view entity is retrieved via a page or a microflow, the corresponding OQL query executes to fetch the relevant data. Consequently, the result set of a view entity is not stored as a separate table in the database (like a materialized view). Instead, the query runs each time the view entity is accessed, dynamically retrieving the data.

During modeling, changes to the underlying entities used in the OQL query affect the options available within the query. At runtime, any changes to the data in the underlying entities immediately impact the data available through the view entity.

View entities can reference other view entities, facilitating more complex structures and better data organization. 

**It is important to note that view entities are read-only.** In order to change the resulting data of a view entity retrieval, the source data should be modified. For this purpose you can set up a microflow to map a view object to object(s) of their corresponding source entity/entities and commit those.

## 2 Enabling View Entities in Your App
To enable view entities in Studio Pro, follow these steps:

1. [TODO]

Once enabled, view entities will appear in the toolbox and toolbar. You can drag and drop them into the domain model. In the dialog that opens, you will be able to define the view entity using an OQL query. The **Attributes** and **Associations** section on the right side will display the query's result set. You can include or exclude certain columns by selecting the checkbox in the **Use** column.

## 3 Access Rules
When the security level of the app is set to **Production**, the **Access Rules** tab will become available in the view entity properties dialog.

Assigning write access to an attribute of a view entity allows the selected module role to edit the in-memory representation of the query result, but not the underlying source entity. The access level set on the view entity is the sole determining factor for whether a role can read or write to it. The access levels of underlying entities are not considered. This is crucial to prevent unintended exposure of data that is restricted at the source entity level.

Although direct writing from the view entity to its source entities is unsupported, you can set up a microflow to retrieve and update the source entities to achieve this functionality.

## 4 Using a View Entity
After creating a view entity in the domain model, it can be used in microflows and pages.
