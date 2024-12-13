---
title: "Security Overview"
url: /refguide/security-overview/
weight: 20
---

{{% alert color="info" %}}
This feature is currently in beta. For more information, see [Beta and Experimental Releases](/releasenotes/beta-features/).
{{% /alert %}}

## Introduction

The **Security Overview** page provides you with a clear and unified overview of your app's security. This overview can be used to review the security of your app. 

## Accessing the Security Overview

To access the **Security Overview** page, open the **App** menu, and then click **Show Security Overview (Beta)**.

## Using the Security Overview

The **Security Overview** summarizes the app's security for a selected user role. This user role can be selected in the dropdown at the top of the overview with the **Show access for user role** label.

In the sidebar of the overview, the current module can be selected. This selection filters the content in the **Entity access**, **Page access**, **Microflow access**, and **Nanoflow access** tabs. 

{{% alert color="info" %}}

The list of modules does not show the System module or any protected modules.

{{% /alert %}}

## Security Overview Tabs

The **Security Overview** is split into four tabs: **Entity access**, **Page access**, **Microflow access** and **Nanoflow access**. As of Studio Pro 10.18.0, only the **Entity access** tab is available. The other tabs will display a *Coming soon!* message.

{{< figure src="/attachments/refguide/modeling/security/app-security/user-roles/security-overview.png" class="no-border" >}}

## Entity Access

The **Entity Access** tab shows a summarized view of the permissions that will be applied during runtime for all entities in the selected module for each user role. This helps developers and reviewers easily understand what an end user can or cannot access within the application.

The **Combined access rules** column aggregates all access rules applicable to the selected user role, reflecting the runtime behaviour. This means that if any access rule grants access to that user, the user will have access. For example, if one access rule grants **Read and Create** access and another access rule grants **ReadWrite** access, the combined access is **ReadWrite** and **Create** access.
Multiple columns will be shown for entities with XPath constraints. Access rules with the same XPath constraint are also combined here, so each XPath in this list is unique. 

When the selected user role has no access to an attribute or an association, it will not be shown in the table. If the selected user role has no access to an entity at all, the entity will not be shown in the **Security Overview**.

For more information on how user and module roles work in Studio Pro, see [User Roles](/refguide/user-roles/) . For more information on how access rules work in Studio Pro, see [Access Rules](/refguide/access-rules/).

## Page, Microflow, and Nanoflow Access

These tabs are not yet available and will be released in the future, they display a *Coming soon!* message.
