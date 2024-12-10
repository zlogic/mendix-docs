---
title: "Security Overview (Beta)"
url: /refguide/security-overview/
weight: 20
---

{{% alert color="info" %}}
The Security Overview is currently a Beta feature introduced in Studio Pro 10.18.0. For more information on experimental features, see [Beta and Experimental Releases](/releasenotes/beta-features/).
{{% /alert %}}

## Introduction

The Security Overview provides you with a clear and unified overview of your application's security. This overview can be used to review the security of your application. The overview can be access via **App** > **Show Security Overview (Beta)**.

## The Security Overview layout: User roles and Modules

The security Overview summarizes the application's security for a selected user role. This user role can be selected in the dropdown at the top of the overview with the "Show access for user role:" label.

In the sidebar of the overview the current module can be selected. This selection filters the  the content in the Entity access, Page access, Microflow access, and Nanoflow access tab. The list of modules will only show modules the user has access to, and does not show the System module or protected modules.

## The tabs within the Security Overview

The Security Overview is split in four tabs: "Entity access", "Page access", "Microflow access" and "Nanoflow access". As of Mendix 10.18.0 only the "Entity access" tab is available. The other tabs will display a "Coming soon!" message.

{{< figure src="/attachments/refguide/modeling/security/app-security/user-roles/security-overview.png" class="no-border" >}}

## Entity access

The Entity access tab shows the combined access rules for all entities within the application for the currently selected user role. Individual access rules and module roles are here all combined into the concrete access the runtime will give an user with the selected user role. An access rule does apply to an user roles when any of the module roles of the access rule are part of the module roles of the user role. 

When combining different access rules the Security Overview followes the same behaviour as the runtime does, meaning that if any access rule defines that a user has been granted access, that user has access. Multiple columns per entity can be shown when XPath constrains apply. Access rules with the same XPath contraint are also combined here so each XPath in this list is unique.

When the selected User Role has no access to an attribute or an association it will not be shown in the table. If the selected User Role has no access to an entity at all the entity will not be shown in the Security Overview.

See [User Roles](/refguide/user-roles/) for more information on how user roles work in Mendix.

## Page, Microflow, and Nanoflow access

These tabs display tab a "Coming soon!" message.