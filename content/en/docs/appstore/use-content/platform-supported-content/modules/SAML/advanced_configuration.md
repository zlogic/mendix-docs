---
title: "Advanced configuration for SAML"
url: /appstore/modules/saml/advanced-configuration
weight: 20
description: "Describes the advanced configuration for the SAML module"
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

This document describes the advanced configurations for the SAML module.

### Multi-tenant Behavior

The resource folder contains a file called *SAMLConfig.properties*. In this file, you can optionally override advanced settings from the SAML module. Usage of this file is optional. When the file does not exist or you do not specify a setting, the module will use its default behavior.
This file contains the documented properties, and example lines show the default values of these options.
With these settings, you can configure the behavior of this module and improve the multi-tenant behavior of your application. For plain SAML authentication, it is best to leave this file unchanged.

### Use a Certificate Issued by a Certificate Authority {#use-ca}

By default the SAML SSO module will use self-signed certificates. It is, however, also possible to use certificates issued by a certificate authority (CA).

SAML SSO supports 2 file formats:

* a PKCS 12 file, which typically has extension .pfx or .p12.
* a jks file.

To use a CA-certificate, upload it as your key store file as described in [Managing the Key Store](/appstore/modules/saml/#keystore).
Remember to do the following:

* set the certificate password in the `KeystorePassword` constant of your app to be able to read the contents of the uploaded key store.
* use an alias for the certificate — this must be the name parameter that is provided when creating the certificate you are uploading. If the values do not match, the SAML module will fall back to using a self-signed certificate instead.
* the value of the configured SP EntityID must match the alias that is included in the uploaded key store.

### Custom Behavior

#### evaluateMultipleUserMatches

The module tries to look up the user that matches the provided user name. When multiple `System.User` records are found, this microflow is always executed.

It is possible to customize this microflow to determine the correct user. Whichever user instance is returned will be signed in to the application (and passed on to any other microflow).

#### CustomUserProvisioning {#customuserprovisioning}

When selecting in the SSO configuration to run the `customUserProvisioning` action (previously known as `CustomLoginLogic`), you can update the new or retrieved user with additional information from the assertion. All the assertions are passed into the microflow in the parameter `AssertionAttributeList`, and these can be transformed and stored in the user record. Also, additional roles can be granted to the users based on the assertion attributes.

#### CustomAfterSigninLogic

After a new session is created for the user, this microflow can be called to copy any data from the previous session to the new session. This microflow behaves similarly to the platform after the sign-in microflow. By using this microflow, it is possible to copy records from the anonymous user to the newly signed-in user.

### Customizing the Login Page

Mendix runtime/system module comes with a default login page. When using SAML with a single IdP, this page is not required.
You need to customize this login page when end-users have different ways of login:

1. If you want to use both Mendix (local) login and SSO login:

    1. Go to the **App** > **Show App Directory in Explorer** > **theme/web** folder (for Mendix versions below 9.0.0, this is the **theme** folder).
    2. Rename `login.html` to `login-without-sso.html`.
    3. Rename `login-with-mendixsso-button.html` to `login.html`.
    4. Open login.html, update the **href** to `/SSO`, and give a button name

    Your app is now configured to use Mendix SSO login.
2. If you want to connect your app with multiple IdPs and the end-user of your app needs to select the IdP to use for login.
    Follow the steps below:

    1. Go to the **App** > **Show App Directory in Explorer** > **\implementation\DiscoveryHandler.java**
    2. Find the template **saml2-discovery-binding.vm** and add your customization.

### Custom Settings

The resources folder contains the *SAMLConfig.properties* file, and through this file, advanced settings can be configured for the module. This file contains the settings along with documentation on the settings. Through this file, it is possible to alter the URLs used as well as how the application behaves in a multi-tenant environment. The file also specifies all the default values and behavior in more detail.

If you are using a custom URL, see [How Do I Get my SAML Metadata or CommunityCommons.GetApplicationUrl to Use the Custom URL?](/developerportal/deploy/custom-domains/#use-custom-url) in the *Custom Domains* documentation.
