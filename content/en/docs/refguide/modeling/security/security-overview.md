---
title: "Security Overview (Beta)"
url: /refguide/security-overview/
weight: 20
---

{{% alert color="info" %}}
The Security Overview is currently a Beta feature introduced in Studio Pro 10.18.0. For more information on experimental features, see [Beta and Experimental Releases](/releasenotes/beta-features/).
{{% /alert %}}

## Introduction

The Security Overview provides you with a clear and unified overview of your application's security.

## The Security Overview layout: User roles and Modules

The security Overview summarizes the application's security for a selected user role. This user role can be selected in the dropdown at the top of the overview with the "Show access for user role:" label.

In the sidebar of the overview the current module can be selected. This selection filters the  the content in the Entity access, Page access, Microflow access, and Nanoflow access tab.

## The tabs within the Security Overview

The Security Overview is split in four tabs: "Entity access", "Page access", "Microflow access" and "Nanoflow access". As of Mendix 10.18.0 only the "Entity access" tab is available. The other tabs will display a "Coming soon!" message.

## Entity access

The Entity access tab shows the combined access rules for all entities within the application for the currently selected user role. Individual access rules and module roles are here all combined into the concrete access the runtime will give an user with this user role. When combining different access rules the Security Overview followes the same behaviour as the runtime does, meaning that if any access rule defines that a user has been granted access, that user has access.

Multiple columns per entity can be shown when XPath constrains apply. Access rules with the same XPath contraint are also combined here so each XPath in this list is unique.

## Page access

Coming soon.

## Microflow access

Coming soon.

## Nanoflow access

Coming soon.