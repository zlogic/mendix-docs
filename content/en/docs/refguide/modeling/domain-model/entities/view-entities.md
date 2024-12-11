---
title: "View Entities"
url: /refguide/view-entities/
weight: 25
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

A view entity represents the result set of a stored [OQL query](/refguide/oql) and can be used similarly to a persistable entity. This concept parallels the function of views in general database technology. Whenever a view entity is retrieved via a page or a microflow, the corresponding OQL query executes to fetch the relevant data. Consequently, the result set of a view entity is not stored as a separate table in the database (like a materialized view). Instead, the query runs each time the view entity is accessed, dynamically retrieving the data.

During modeling, changes to the underlying entities used in the OQL query affect the options available within the query. At runtime, any changes to the data in the underlying entities immediately impact the data available through the view entity.

View entities can reference other view entities, facilitating more complex structures and better data organization. 

**It is important to note that view entities are read-only.** In order to change the resulting data of a view entity retrieval, the source data should be modified. For this purpose you can set up a microflow to map a view object to object(s) of their corresponding source entity/entities and commit those.

## Enabling OQL version 2 in Your App
In order to use view entities, your app must use OQL version 2. You can set this yourself by changing the setting in **App** > **Settings** > **Runtime** and set the toggle [OQL version 2](/refguide/app-settings/#oql-version-2) to **Yes**. You can also drag a new view entity from the toolbar or the toolbox to the domain model, upon which Studio Pro will ask you to confirm setting the OQL version to v2. 

## General

The **General** tab contains the OQL Query editor, and the Preview data table.

The **OQL Query** editor allows you to write the query that defines this view entity. While writing this query, it helps you to write a correct query by suggesting the names of the entities and attributes in your domain model and allowed operators and functions. If the query is not valid, a list of validation errors will be displayed underneath the editor with the line and column number of the place where it found the error.

The resulting names and types of the attributes of your view entity will be displayed as column headers in the **Preview data** table. This table allows you to view the resulting data ser of your OQL query, by clicking **Run Query**. For this button to be enabled, your app must be running. When clicking **Run Query**, Studio Pro will retrieve the data from the database that is configured in your app settings. The database type of the active configuration is also listed in the header of the Preview data section.

{{% alert color="info" %}}
The Preview data table tries to retrieve the data using your OQL query from the running app. That means that, if you have changed your domain model since you last started the app, you can run into errors when your OQL query uses attributes or entities that are not yet existing in the version of the app that is currently running.
{{% /alert %}}

## Access Rules
When the security level of the app is set to **Production**, the **Access Rules** tab will become available in the view entity properties dialog.

Assigning write access to an attribute of a view entity allows the selected module role to edit the in-memory representation of the query result, but not the underlying source entity. The access level set on the view entity is the sole determining factor for whether a role can read or write to it. The access levels of underlying entities are not considered. This is crucial to prevent unintended exposure of data that is restricted at the source entity level.

Although direct writing from the view entity to its source entities is unsupported, you can set up a microflow to retrieve and update the source entities to achieve this functionality.

## Documentation

Here you can add a description of the view entity. This is available to other users working on this app.

## Using a View Entity
After creating a view entity in the domain model, it can be used in microflows and pages like any other entity.
