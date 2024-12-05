---
title: "Private Mendix Platform Functionalities - System Administrators"
url: /private-mendix-platform/reference-guide/admin/system/
description: "Provides details on the features and functionality of the Private Mendix Platform that are available to system administrators."
weight: 20
---

## Introduction

In Private Mendix Platform, system administrators primarily manage key settings which must be configured during the initial implementation, and which are rarely modified during normal operation. Settings relevant for systems admins are available in the [Settings](#settings) section of the admin navigation menu.

## Accessing the Configuration Settings

As a user with system administrator access rights, you can access the Private Mendix Platform configuration settings by performing the following steps:

1. Switch to Admin Mode by clicking the profile picture in the top right corner of the screen and selecting **Switch to Admin Mode**.
2. In the left navigation menu, open the **Settings** section.

## Settings {#settings}

The **Settings** section of the administrator navigation menu contains setting relevant to your day-to-day tasks as a system admin. You can use it to manage your branding, license, Marketplace settings, and version control settings.

Some of the settings that you configure here are initially set by the [Private Platform Configuration Wizard](/private-mendix-platform/quickstart/#wizard). System administrators can also update them at any time after the initial configuration.

### Preferences

General configuration settings allow you to manage the basic aspects of your Private Mendix Platform, such as the platform name and branding, toggling certain capabilities on or off, and version support settings. The settings in this section are largely configured when you run the initial configuration wizard, but you can still review and adjust them later during the implementation process.

#### General

The **General** tab allows you to configure information about your organization, and optionally also the Certified Mendix Partner that is working with you on implementing Private Mendix Platform. You can also use it to configure your locale settings.

{{% alert color="info" %}}
Changing your locale sets locale-dependent formats, such as date and time, to the preferred format of the selected locale. The settings are applied to the Private Mendix Platform (for example, in the Marketplace or Mendix Portal), not in the apps created through the Platform.
{{% /alert %}}

##### Branding

The settings in this section allow you to configure custom branding for your Private Mendix Platform. You can customize the title of the Platform as shown in the top bar, upload your logo, or change the image on the login page.

{{< figure src="/attachments/private-platform/pmp-wizard1.png" class="no-border" >}}

##### Support

In this section, you can provide your own help and support instructions for users of your Private Mendix Platform.

{{< figure src="/attachments/private-platform/pmp-wizard1.png" class="no-border" >}}

Users can then see these instructions on the **Logs and Events** page for their app.

##### Export Settings

...

#### Notifications

...

#### Marketplace

For Private Mendix Platform, the Marketplace is also private and hosted entirely within the platform itself. The settings in this section allow you to configure the administrative settings for publishing and downloading content to and from the private Marketplace.

##### Content Approvals

In this tab, you can configure whether contents that users publish to the private Marketplace requires administrator approval before publishing. To view all [pending, published, and rejected content items](/private-mendix-platform/reference-guide/admin/company/#content), click **Go to Marketplace Management**.

##### Content Import {#import}

You can populate your private Marketplace with contents by importing a zip file that contains the content packages along with a *package.json* file. You can upload the file from a Content Delivery Network, or manually from your local machine.

{{< figure src="/attachments/private-platform/pmp-admin9.png" class="no-border" >}}

###### Manually Importing Marketplace Content

To manually upload a content bundle from your own computer, perform the following steps:

1. Download the Marketplace Bundle with contents available in a zip file. If you do not have access to the bundle, contact your Mendix point of contact.
2. Click **Upload Marketplace Bundle** to go to the **Import Content** > **Upload Marketplace Bundle** tab.
3. Follow the steps described in [Company Administrators](/private-mendix-platform/reference-guide/admin/company/#manual-upload).

###### Importing Marketplace Content from a CDN {#configure-import}

To enable content import from a Content Delivery Network, follow these steps:

1. Download the Marketplace Bundle with contents available in a zip file. If you do not have access to the bundle, contact your Mendix point of contact.
2. Unzip the files to an internal location which Private Mendix Platform can access via HTTP or HTTPS. Do not change the directory structure.
3. If using a self-signed certificate for your internal locations, configure Mendix Operator to trust your private Certificate Authorities. For more information, see [Creating a Private Cloud Cluster](/developerportal/deploy/standard-operator/#custom-tls).
4. In the **Content Import** tab, in the **Marketplace import bundle URL** field, enter the root URL of the *package.json* file included in the Marketplace download. 

    For example, if the *package.json* can be accessed at the URL `https://<your domain>/release/marketplace/Marketplace-1.0/package.json`, enter the following URL: `https://<your domain>/release/marketplace/Marketplace-1.0/`.

    {{< figure src="/attachments/private-platform/pmp-config3.png" class="no-border" >}}

5. Set the **Authentication** toggle to **ON**, and then specify the user name and password required to download the bundle.
6. Click **Save** to enable content import from this bundle.
7. Click **Go to Marketplace Import** to view the available downloads in the **Import Content** > **Import from CDN** tab.

#### Version Support

...

### Integrations

...

#### Identity & Access

In this section, you can configure SSO authentication for your users logging in to Private Mendix Platform. OIDC and SAML are supported as protocols.

##### IdP Integration (OIDC)

You can configure SSO authentication with the OIDC protocol. For more information, see [Runtime Configuration of Your IdP at Your App](/appstore/modules/oidc/#runtime-idp-app).

##### IdP Integration (SAML)

To configure SSO authentication with the SAML protocol, first [configure the service provider](/appstore/modules/saml/#configure-sp) in the **SP Configuration** tab, and then [create the IdP-specific settings](/appstore/modules/saml/#idp-specific-settings) in the **IdP Configuration** tab.

To [debug the configuration](/appstore/modules/saml/#debugging-the-configuration), you can view the log files in the **Log** tab.

##### OIDC Provider

The settings under this tab control the connection between Studio Pro and the platform. They should not be changed without advanced knowledge of the platform. Stop and restart the Private Platform portal if you are having trouble logging in with Studio Pro.

##### SCIM Provisioning

System for Cross-Domain Identity Management (SCIM) is a protocol that simplifies user access management for applications. Private Mendix Platform uses the SCIM standard to pre-provision selected users onto your Platform without the users having to manually log in through SSO first.

To enable SCIM provisioning, perform the following steps:

1. Log in to Private Mendix Platform as an administrator.
2. In the **Authentication** section, click the **IdP Integration (OIDC)** or the **IdP Integration (SAML)** tab.
3. Edit your IdP configuration, and then click the **Provisioning** tab.
4. In the **Just in time provisioning** section, map the IdP attributes to the matching Mendix object attributes.
5. In the **Authentication** section, click the **SCIM Provisioning** tab, and then click **New**.
6. In the **IDP Configuration Page** dialogue, enter a name for the connection, and obtain the token for your identity provider by clicking **Copy**.
7. Enter the token in the configuration panel of your identity provider and verify that the connection is working.

##### MxAdmin Settings

By default, the platform has a default system administrator account called MxAdmin. You can disable the account by setting the **Disable MxAdmin** toggle to **Yes**.

{{% alert color="info" %}}
Ensure that you have at least one other user with the System Administrator role assigned before disabling MxAdmin.
{{% /alert %}}

##### Preferences

You can configure the following preferences for login sessions in Private Mendix Platform:

* **Inactivity Period for Automatic Account Disabling (Hours)** - The number of hours after which an unused account is disabled; if set to 0, accounts are not automatically disabled
* **Maximum Concurrent Sessions Per User Account** - The maximum number of concurrent login sessions that users can have; if set to 0, logging in while another session is running (for example, on a different browser or machine) ends the previous session and logs the user off
* **Failed Login Attempts to Lockout** - The number of failed login attempts after which the user account is locked for the duration specified below; if set to 0, accounts are not automatically locked out
* **Account Lockout Duration (Minutes)** - The number of minutes after which a locked out account is reactivated; if set to 0, locked out accounts must be reactivated by an administrator

By default, all of these options are disabled (that is, set to a *0* value). To enable any of them, enter a number greater than zero into the corresponding field.

#### Project Management

...

#### Version Control

To create applications and collaborate, configure the connection to your version control repository. GitHub, GitLab, Azure DevOps, and Bitbucket are supported as version control systems. For more information, see [Configuring the Version Control System for Private Mendix Platform](/private-mendix-platform-version-control/).

#### Build

...

#### Deployment

...

### Advanced

In this section, you can adjust the advanced configuration settings of your Private Mendix Platform.

#### Capabilities

The settings in this section allow you to configure the basic aspects of your Private Mendix Platform:

* **Enable App Projects?** - Recommended. Enables you to create and manage your app projects. Enables app projects and related settings across the portal. Must be enabled for CI/CD capabilities.

* **Enable Marketplace?** - Recommended. Enables you to use the Private Platform's Marketplace capabilities to upload, import and manage Marketplace contents. The Marketplace enabled here is hosted entirely within your Private Mendix Platform.

* **Enable Build and Deploy** - Recommended. Enables you to use the Private Platform's CI/CD capabilities to build and deploy apps. Enables the Build and Deploy pipeline, environments, metrics, logging, and related settings.

* **Enable Identity & Access Integration?** - Optional. Enable users to log in using SSO by configuring your IdP integration.

* **Allow sign up?** - Optional. Enable users to log in with a local user account, instead of or in addition to SSO.

* **Enable Webhooks?** - Optional. Webhooks allow to send information between platform and external systems, and can be triggered by events around Apps, Users, Groups, Marketplace and CI/CD.

* **Enable License Management?** - Recommended. Upload your license bundle to automatically provision app licenses through Private Cloud License Manager. For more information, see Private Cloud License Manager.

#### Operational

In this section, you can access the list of scheduled events and the Mx Model Reflection tool.

##### Scheduled Event

This tab shows a list of all the scheduled tasks and actions in the system, together with start time, end time, and status.

{{< figure src="/attachments/private-platform/pmp-wizard6.png" class="no-border" >}}

##### Mx Model Reflection

For more information about this platform-supported module, refer to [Mx Model Reflection](/appstore/modules/model-reflection/).


## Email Settings

Email settings allow you to manage your the SMTP server settings used by Private Mendix Platform. These settings are necessary to ensure that your system can send out email notifications. You can also configure additional settings such as email templates, view your email queue, and manage recurring tasks.

### Templates

In this tab, you can create and manage the templates for any standard notification emails that you want your app to send, such as automated reports, assigned tasks, or others. Templates created here can then be referenced in microflows.

{{< figure src="/attachments/private-platform/pmp-wizard3.png" class="no-border" >}}

### Emails

In this tab, you can view the following details about the emails sent from your system:

* **Queued** - A list of all emails queued to be sent, regardless of delivery status.
* **Sent** - A list of all emails that were successfully sent.
* **Failed** - A list of emails that could not be sent after a maximum number of attempts defined in the Configuration tab.
* **Logs** - Errors and other messages that were logged while attempting to send emails. You can search the list by date, message type and content, or the microflow that triggered the email.

### Configuration

In this tab, you can configure SMTP server settings for your email account.

{{< figure src="/attachments/private-platform/pmp-wizard4.png" class="no-border" >}}

### Administrative Tasks

In this tab, you can trigger various scheduled tasks, such as sending queued emails or cleaning the email queue.



## Mx Version Settings

In this section, you can view or disable the versions of Mendix Studio Pro that your users are allowed to download.



### CI/CD

Configure CI/CD capabilities for your app. If you enable this option, you must also specify your CI system, configure the necessary settings, and register a Kubernetes cluster. Tekton, Jenkins, [AzureDevops](/private-mendix-platform/configure-azure/) and [Kubernetes](/private-mendix-platform-configure-k8s/) are supported. You can also configure a custom template for your CI/CD capabilities.




