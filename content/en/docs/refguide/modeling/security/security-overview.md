---
title: "Security Overview (Beta)"
url: /refguide/security-overview/
weight: 20
---

{{% alert color="info" %}}
The Security Overview is currently a Beta feature introduced in Studio Pro 10.18.0. For more information on experimental features, see [Beta and Experimental Releases](/releasenotes/beta-features/).
{{% /alert %}}

## Introduction

The Security Overview provides you with a clear and unified overview of your application's security. This overview can be used to review the security of your application. The overview can be accessed via **App** > **Show Security Overview (Beta)**.

## The Security Overview layout: User roles and Modules

The security Overview summarizes the application's security for a selected user role. This user role can be selected in the dropdown at the top of the overview with the "Show access for user role:" label.

In the sidebar of the overview, the current module can be selected. This selection filters the content in the Entity access, Page access, Microflow access, and Nanoflow access tab. The list of modules does not show the System module or any protected modules.

## The tabs within the Security Overview

The Security Overview is split into four tabs: "Entity access", "Page access", "Microflow access" and "Nanoflow access". As of Mendix 10.18.0, only the "Entity access" tab is available. The other tabs will display a "Coming soon!" message.

{{< figure src="/attachments/refguide/modeling/security/app-security/user-roles/security-overview.png" class="no-border" >}}

## Entity access

The Entity Access tab shows a summarized view of the permissions that will be applied during runtime for all entities in the selected module for each user role. This helps developers and reviewers easily understand what an end user can or cannot access within the application.

The `Combined access rules` column aggregates all access rules applicable to the selected user role, reflecting the runtime behaviour. This means that if any access rule grants access to that user, the user will have access. For example, if one access rule grants Read and Create access and another access rule grants ReadWrite access, the combined access is ReadWrite and Create access.
Multiple columns will be shown for entities with XPath constraints. Access rules with the same XPath constraint are also combined here, so each XPath in this list is unique. 

When the selected user role has no access to an attribute or an association, it will not be shown in the table. If the selected user role has no access to an entity at all, the entity will not be shown in the Security Overview.

* See [User Roles](/refguide/user-roles/) for more information on how user and module roles work in Mendix.
* See [Access Rule"s](/refguide/access-rules/) for more information on how access rules work in Mendix.

## Page, Microflow, and Nanoflow access

These tabs display tab a "Coming soon!" message.