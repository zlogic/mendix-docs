---
title: "Private Mendix Platform Functionalities - Administrators"
linktitle: "Administrators"
url: /private-mendix-platform/reference-guide/admin/
description: "Provides details on the features and functionality of the Private Mendix Platform that are available to administrators."
weight: 10
aliases:
    - /private-mendix-platform-administration/
    - /private-mendix-platform/administration/
---

## Introduction

This section of the Private Mendix Platform Reference Guide provides information about the menus and functionalities of the Private Mendix Platform that are available to administrator users. As an administrator, you can access the options described in this section by clicking on your user icon in the upper right corner of the screen, and then selecting **Switch to Admin Mode**.

Private Mendix Platform distinguishes between the following types of administrator roles:

* Company admin - This role primarily manages business-as-usual everyday tasks, for example inviting new users to an app, or approving Marketplace contents. Settings relevant for company admins are available in the [Manage](#manage) section of the admin navigation menu.
* Systems admin - This role primarily manages key settings which must be configured during the initial implementation, and which are rarely modified during normal operation. Settings relevant for systems admins are available in the [Settings](#settings) section of the admin navigation menu.

{{< figure src="/attachments/private-platform/pmp-admin4.png" class="no-border" >}}

## Manage {#manage}

The **Manage** section of the administrator navigation menu contains setting relevant to your day-to-day tasks as a company admin. You can use it to manage your company apps, users and groups, Marketplace contents, and deployment clusters.

### Apps

The **Apps** section of the navigation menu lets administrators manage their apps.

#### App Management

On the **App Management** page, you can view a summary of your apps.

{{< figure src="/attachments/private-platform/pmp-admin1.png" class="no-border" >}}

Click an app tile to see more details about the app.

By clicking **More Actions** ({{% icon name="three-dots-menu-horizontal" %}}) in the app tile, you can quickly perform a number of actions:

##### Editing App details

...

##### Assigning the App to a New owner

...

##### Sharing the App with Selected User groups

...

##### Archiving the App

...

You will be warned of the consequences and asked for confirmation before the app is archived.

##### Deleting the App

...

You will be warned of the consequences and asked for confirmation before the app is archived.

#### Import Apps

...

### Marketplace

In the **Marketplace** section, administrators can manage various settings related to the content available on the Private Platform Marketplace. The Private Platform Marketplace is a local version of the [Mendix Marketplace](/appstore/overview/), enclosed entirely within the Private Platform. Developers in your organization can also create their own modules, connectors, and sample apps, and share them on the Private Platform Marketplace to make them available to other users.

{{< figure src="/attachments/private-platform/pmp-admin2.png" class="no-border" >}}

As the administrator, you can perform the following actions:

* In the **Content Management** tab, you can view the Marketplace content that your users have already published, as well as any items which are pending approval, or which have been rejected.
* In the **Taxonomy Management** tab, you can configure the supported Studio Pro versions and sub-categories that your users can select when creating Marketplace content. You can also view and edit the available licenses.
* In the **Content Import** tab, you can view the contents available in your Private Marketplace. You can also download and import the modules in bulk.

#### Content Management

...

#### Taxonomy Management

...

#### Import Content

...

### Deployment

In the **Deployment** section, administrators can manage existing clusters and register new ones.

#### Cluster Manager

...

### Users

In the **User Management** section, administrators can manage user accounts and user groups.

{{< figure src="/attachments/private-platform/pmp-admin3.png" class="no-border" >}}

As the administrator, you can perform the following actions:

* In the **User Management** tab, you can create and edit accounts for your local users and API users (that is, accounts that are used by an API service to access your Private Mendix Platform). By clicking **More Actions** ({{% icon name="three-dots-menu-horizontal" %}}) by an account, you can quickly perform a number of actions:

    * Edit a user's name and email
    * Assign or remove user roles
    * Block a user
    * Change a user's password
    * Configure the language and time zone settings for a user
    * Delete a user account

* In the **Group Management** tab, you can create and edit user groups. These groups typically reflect your organization's structure. You can also use the **Automation Settings** option to automatically assign users to groups based on their profile attributes.

#### User Management

...

#### Group Management

...

### Platform

In the **Deployment** section, administrators can view and manage statistics, activity logs, webhooks, and licenses.

As the administrator, you can perform the following actions:

* In the **Platform Statistics** tab, you can access statistics such as the number of users and apps, daily user login times and numbers, or most active users.
* In the **Platform Logs** tab, you can view a log of actions performed by users, for example, creating and deleting apps, starting a pipeline, or adding a new user.
* In the **Webhooks** tab, you can view and manage your [Webhooks](/developerportal/deploy/webhooks/).
* In the **Licensing** tab, you can check the status of your licenses, or upload a new Private Mendix Platform license bundle.

#### Platform Statistics

...

#### Activity Logs

...

#### Webhooks

...

#### Licensing

...

## Settings {#settings}

...

### Preferences

...

#### General

...

#### Notifications

...

#### Marketplace

...

#### Version Support

...

### Integrations

...

#### Identity & Access

...

#### Project Management

...

#### Version Control

...

#### Build

...

#### Deployment

...

### Advanced

...

#### Capabilities

...

#### Operational

...