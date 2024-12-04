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

{{< figure src="/attachments/private-platform/pmp-admin5.png" class="no-border" >}}

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

By clicking **More Actions** ({{% icon name="three-dots-menu-horizontal" %}}) in the **Action** column, you can quickly perform a number of actions:

* [Edit app details](#app-details)
* [Assign the app to a new owner](#ownership)
* [Share the app with groups](#share)
* [Archive the app](#archive)
* [Delete the app](#delete)

##### Editing App Details {#app-details}

Company admins can edit app details such as the app branding or ownership, team membership, and others.

1. In the **App Management** page, in the **Action** column, click **More Actions** > **Edit**.
2. Configure any of the following settings:

* **App Details** - Configure the following basic app details:
    * **App ID** - An automatically assigned internal ID. This value cannot be adjusted.
    * **Name** - The name of your app. This value should be meaningful, so that users can easily identify the app.
    * **Description** - An optional description of the application.
    * **App Logo** - A browser logo for your app. This value can only be adjusted by the app owner.
* **Team Membership** - Invite users to join your app, or remove them from the app. Before you can invite a user, you must first configure their account on the [User Management](#users) page.
* **Git Server** - View the revisions per branch for this app.
* **Ownership and Sharing** - Select the user and optionally the group that owns this app. If an app is owned by a specific group, the owner must be a user who belongs to that group.

##### Assigning the App to a New Owner {#ownership}

Each app is owned by a specific user, who can change settings such as the application branding (logo). By default, the owner is the user who created the app. To transfer the ownership to another user, on the **App Management** page, in the **Action** column, click **More Actions** > **Transfer Ownership**. Alternatively, you can access the same option in the **Ownership and Sharing** tab of the [App Details](#app-details) page.

In addition to specifying the owner, you can also assign ownership of the app to a specific group. If an app is owned by a group, the owner must be a user who belongs to that group.

##### Sharing the App with Selected User Groups {#share}

You can share your app with any user groups by selecting the **More Actions** > **Share with Groups** option from the **App Management** page.

##### Archiving the App {#archive}

If an app is not needed for a time, you can archive it. An archived app is no longer available to users, but its data is still stored, and it can be quickly un-archived if required.

To archive an app, on the **App Management** page, in the **Action** column, click **More Actions** > **Archive App**. To reactivate an archived app, select **Unarchive App** from the same menu. Alternatively, you can access the same options on the [App Details](#app-details) page. The app is archived or unarchived immediately.

##### Deleting the App {#delete}

If the app is no longer needed and you do not want to store its data, you can delete it. Deleting an app removes its repository, so that it cannot be restored afterwards. To delete an app, click **Delete App** on the [App Details](#app-details) page.

You will be warned of the consequences and asked for confirmation before the app is deleted.

#### Import Apps

On the **Import Apps** page, administrators can import existing Mendix apps that are in their version control host but not yet in Private Mendix Platform. The import currently supports the following hosts:

* GitLab
* GitHub
* Bitbucket

{{< figure src="/attachments/private-platform/pmp-admin6.png" class="no-border" >}}

The list of apps on the page is not refreshed automatically. To refresh it, click the **Scan for Apps** button.

### Marketplace

In the **Marketplace** section, administrators can manage various settings related to the content available on the Private Platform Marketplace. The Private Platform Marketplace is a local version of the [Mendix Marketplace](/appstore/overview/), enclosed entirely within the Private Platform. Developers in your organization can also create their own modules, connectors, and sample apps, and share them on the Private Platform Marketplace to make them available to other users.

As the administrator, you can perform the following actions:

#### Content Management

In the **Content Management** tab, you can view the Marketplace content that your users have already published, as well as any items which are pending approval, or which have been rejected.

{{< figure src="/attachments/private-platform/pmp-admin7.png" class="no-border" >}}

You can click on an item to view more information about it, download, approve, or delete it.

#### Taxonomy Management

In the **Taxonomy Management** tab, you can configure the supported Studio Pro versions and sub-categories that your users can select when creating Marketplace content. You can also view and edit the available licenses.

{{< figure src="/attachments/private-platform/pmp-admin8.png" class="no-border" >}}

##### Supported Versions

The versions listed on the **Supported Versions** page indicate the Mendix Studio Pro versions that a Marketplace content item is compatible with. Users can select these versions from a list when uploading new Marketplace content.

##### Licenses

The **Licenses** tab allows administrators to specify the licenses that are available to their apps. To add a new license, click **New License** and specify the following values:

* **Name** - Name of the license
* **URL** - URL at which the license can be accessed
* **Contents** - Optional additional information about the license.

##### App Categories

In the **App Categories** tab, you can view the predefined categories that users can specify for their Marketplace content items. You can also add custom categories, for example, for Marketplace contents related to a specific project.

Predefined app categories cannot be edited or deleted. To edit or delete a custom category, click the **Action** menu, and then select one of the available options.

#### Import Content

You can populate your private Marketplace with contents by importing a zip file that contains the content packages along with a *package.json* file. You can upload the file from a Content Delivery Network, or manually from your local machine.

{{< figure src="/attachments/private-platform/pmp-admin9.png" class="no-border" >}}

#### Upload Marketplace Bundle

To manually upload a content bundle from your own computer, perform the following steps:

1. Download the Marketplace Bundle with contents available in a zip file. If you do not have access to the bundle, contact your Mendix point of contact.
2. In the **Import Content** > **Upload Marketplace Bundle** tab, drag and drop the file that you want to upload.

    * The file must be in *zip* format.
    * The file must not be larger than 2048 MB.
    * Your infrastructure must support the upload of large files (up to 2048MB).
    * You should also have at least 40 GB available disk space to account for temporary files.
    

3. Click **Import Marketplace Bundle components**.

    {{< figure src="/attachments/private-platform/pmp-config1.png" class="no-border" >}}

4. To view the progress of your upload, click **View Task Queue**.

{{% alert color="info" %}}
If you are experiencing high latency during manual uploads, you can increase the timeouts. For example, for nginx, you can perform the following commands:

```text
nginx.ingress.kubernetes.io/client-header-timeout: "300"
nginx.ingress.kubernetes.io/proxy-connect-timeout: "300"
nginx.ingress.kubernetes.io/proxy-read-timeout: "300"
nginx.ingress.kubernetes.io/proxy-send-timeout: "300"
```

{{% /alert %}}

#### Import from CDN

To enable content import from a Content Delivery Network, follow these steps:

1. Download the Marketplace Bundle with contents available in a zip file. If you do not have access to the bundle, contact your Mendix point of contact.
2. Unzip the files to an internal location which Private Mendix Platform can access via HTTP or HTTPS. Do not change the directory structure.
3. If using a self-signed certificate for your internal locations, configure Mendix Operator to trust your private Certificate Authorities. For more information, see [Creating a Private Cloud Cluster](/developerportal/deploy/standard-operator/#custom-tls).
4. In the **Import Content** tab, in the **Marketplace import bundle URL** field, enter the root URL of the *package.json* file included in the Marketplace download. 

    For example, if the *package.json* can be accessed at the URL `https://<your domain>/release/marketplace/Marketplace-1.0/package.json`, enter the following URL: `https://<your domain>/release/marketplace/Marketplace-1.0/`.

    {{< figure src="/attachments/private-platform/pmp-config3.png" class="no-border" >}}

5. Set the toggle **Enable content import with external source** to **ON**.
6. Click **Save** to enable content import from this bundle.
7. In the **Import Content** > **Import from CDN** tab, you can now view the available downloads.

### Deployment

In the **Deployment** section, administrators can manage existing clusters and register new ones.

#### Cluster Manager

...

### Users {#users}

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