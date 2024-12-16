---
title: "Build a Mendix Native App in the Cloud with Bitrise"
linktitle: "Deploy Mendix Native Mobile App with Bitrise"
url: /refguide/mobile/distributing-mobile-apps/building-native-apps/native-deploy-with-bitrise/
weight: 30
description: Describes how to itegrate with Bitrise to build a Mendix native app in the cloud.
---

## Introduction

{{% alert color="warn" %}}
Microsoft announced that Visual Studio App Center will be retired on March 31, 2025 ([Find the announcement here](https://learn.microsoft.com/en-us/appcenter/retirement)). This guide introduces an alternative approach using [Bitrise](https://bitrise.io) for building and deploying Mendix Native apps in the cloud. This guide provides steps for transitioning to Bitrise.
{{% /alert %}}

This guide helps you set up Bitrise to automate building, testing, and deploying Mendix Native apps in the cloud. This enables you to streamline your CI/CD processes for greater ease of use at scale.

## Prerequisites {#prerequisites}

Before you begin, ensure you:

- Install the [latest MTS version](/releasenotes/studio-pro/lts-mts/#mts) of Mendix Studio Pro using the online installer.
- Complete [Get Started with Native Mobile](/refguide/mobile/getting-started-with-mobile/) to create, style, and debug an application with Mendix Studio Pro.
- Upload your native mobile project output to git via Studio Pro, and have the cloud address of your uploaded project ready.
- Possess accounts for [GitHub](https://github.com/) or other Git provider and [Bitrise](https://bitrise.io).

### Platform-Specific Prerequisites

For iOS deployment, complete these prerequisites:

- Register for an Apple Developer Account.
- Have an iOS device for testing the iOS package.
- Have an iOS deployment certificate and provisioning profile for testing.

For Android deployment, you only need to have an Android device available.

## Bitrise Workflow Configuration

The following YAML file configures a `deploy` workflow for building, testing, and deploying Mendix apps using Bitrise:

```yaml
format_version: '13'
default_step_lib_source: https://github.com/bitrise-io/bitrise-steplib.git
project_type: react-native

workflows:
  deploy:
    description: |
      Tests, builds, and deploys the app using the *Deploy to bitrise.io* step.

      Next steps:
      - Set up an [Apple service with API key](https://devcenter.bitrise.io/en/accounts/connecting-to-services/connecting-to-an-apple-service-with-api-key.html).
    steps:
    - activate-ssh-key@4:
        run_if: '{{getenv "SSH_RSA_PRIVATE_KEY" | ne ""}}'
    - git-clone@8: {}
    - restore-npm-cache@2.3.1: {}
    - nvm@1.3.3:
        inputs:
        - node_version: '18'
    - npm@1.1.6:
        inputs:
        - command: i
    - npm@1.1.6:
        inputs:
        - command: run configure
    - save-npm-cache@1.2.0: {}
    - install-missing-android-tools@3:
        inputs:
        - gradlew_path: "$PROJECT_LOCATION/gradlew"
    - android-build@1:
        inputs:
        - project_location: "$PROJECT_LOCATION"
        - module: "$MODULE"
        - variant: "$VARIANT"
    - sign-apk@2: {}
    - manage-ios-code-signing@2.0.1: {}
    - restore-cocoapods-cache@2: {}
    - cocoapods-install@2.4.2:
        inputs:
        - source_root_path: "$BITRISE_SOURCE_DIR/ios"
    - save-cocoapods-cache@1: {}
    - xcode-archive@5:
        inputs:
        - project_path: "$BITRISE_PROJECT_PATH"
        - scheme: "$BITRISE_SCHEME"
        - distribution_method: "$BITRISE_DISTRIBUTION_METHOD"
        - configuration: Release
        - automatic_code_signing: api-key
    - deploy-to-bitrise-io@2: {}

meta:
  bitrise.io:
    stack: osx-xcode-16.0.x
    machine_type_id: g2-m1.4core
app:
  envs:
  - opts:
      is_expand: false
    PROJECT_LOCATION: android
  - opts:
      is_expand: false
    MODULE: app
  - opts:
      is_expand: false
    VARIANT: appstoreRelease
  - opts:
      is_expand: false
    BITRISE_PROJECT_PATH: "./ios/NativeTemplate.xcworkspace"
  - opts:
      is_expand: false
    BITRISE_SCHEME: nativeTemplate
  - opts:
      is_expand: false
    BITRISE_DISTRIBUTION_METHOD: development
trigger_map:
- push_branch: main
  workflow: primary
- pull_request_source_branch: "*"
  workflow: primary
```

## Adding Environment Variables and Secrets in Bitrise

For a successful Bitrise setup, you need to configure both environment variables and secrets. Environment variables can be added in the **Env Vars** section, while sensitive information should be added in **Secrets** for security.

### Adding Environment Variables in Bitrise

To add environment variables in Bitrise, do the following:

1. Go to your [Bitrise dashboard](https://app.bitrise.io/).
1. Open your project and navigate to **Workflow** > **Env Vars**.
1. Add the following environment variables:

    - **PROJECT_LOCATION**: Location of the Android project (for example, `android`).
    - **MODULE**: Module name within the Android project (for example, `app`).
    - **VARIANT**: Build variant for Android (for example, `appstoreRelease`).
    - **BITRISE_PROJECT_PATH**: Path to the Xcode workspace or project (for example, `./ios/NativeTemplate.xcworkspace`).
    - **BITRISE_SCHEME**: The scheme name for your iOS project (for example, `nativeTemplate`).
    - **BITRISE_DISTRIBUTION_METHOD**: Distribution method for iOS builds (`development`, `app-store`, etc.).

Each variable should be added as a separate entry in the **Env Vars** section.

### Adding Secrets in Bitrise

Sensitive credentials should be added in the **Secrets** section to keep them secure. Do so as follows:

1. Go to **Workflow** > **Secrets**.
1. Add the following secrets:

    - **GIT_HTTP_USERNAME**: Username for Git HTTP authentication.
    - **GIT_HTTP_PASSWORD**: Personal access token for Git HTTP authentication.
    - **MYAPP_RELEASE_KEY_ALIAS**: Alias for the Android release key.
    - **MYAPP_RELEASE_STORE_PASSWORD**: Password for the Android keystore.
    - **MYAPP_RELEASE_KEY_PASSWORD**: Password for the Android key alias.
    - **SSH_RSA_PRIVATE_KEY**: Required to authenticate with Git. Add your private SSH key here.
    - **BITRISEIO_ANDROID_KEYSTORE_URL**: URL to download your Android keystore file.

1. Ensure **Protect variable** is checked for each secret to keep sensitive information secure.

## Additional Setup: Adding Keystore and Apple Service Connections

To build and deploy iOS and Android applications, you must configure code signing and service connections for Apple and Google services in Bitrise.

### Adding a Keystore for Android

To sign your Android app, upload your keystore file to Bitrise:

1. Go to **Workflow** > **Code Signing** > **Android Keystore** in your Bitrise project.
1. Click **Upload keystore file** and select your `.keystore` file.
1. After uploading, provide the following details:
    - **Keystore password**: The password to unlock your keystore file.
    - **Key alias**: The alias name for your key.
    - **Key password**: The password for the key alias.

These fields correspond to the following secrets, which you should add in **Workflow** > **Secrets**:
* `MYAPP_RELEASE_STORE_PASSWORD`
* `MYAPP_RELEASE_KEY_ALIAS`
* `MYAPP_RELEASE_KEY_PASSWORD`

### Connecting to Apple Services

For iOS code signing and deployment, you need to connect Bitrise to your Apple Developer account using an API key:

1. Follow the instructions provided in the Bitrise guide [Connecting to an Apple Service with API Key](https://devcenter.bitrise.io/en/connectivity/apple-services-connection/connecting-to-an-apple-service-with-api-key.html).
1. Go to **Workflow** > **Code Signing** > **Apple Service Connection** and select **Add New**.
1. Upload your API key and provide the **Issuer ID** and **Key ID** from your Apple Developer account.
1. After connecting, Bitrise will use this API key for automatic code signing in your iOS builds.

### Exporting iOS Code Signing Certificates

If you do not have your certificates on hand, export them from Xcode:

1. Open Xcode and go to **Preferences** > **Accounts**.
1. Select your **Apple ID** and click **Manage Certificates**.
1. Find your **iOS Distribution Certificate**, right-click, and choose **Export Certificate**.
1. Save it as a *.p12* file and upload it to Bitrise under **Workflow** > **Code Signing** > **iOS Certificates**.

For detailed steps, follow Bitrise's guide on [Exporting iOS Code Signing Certificates](https://devcenter.bitrise.io/en/code-signing/ios-code-signing/exporting-ios-code-signing-files-without-codesigndoc.html#exporting-ios-code-signing-certificates-with-xcode).

### Connecting a Google Service Account (Optional for Google Services)

If your app requires Google services, such as Firebase or Google Play, you need to connect a Google Service Account to Bitrise:

1. Create a **Service Account** in your [Google Cloud Console](https://console.cloud.google.com/).
1. Download the service account key file as JSON.
1. In Bitrise, go to **Workflow** > **Secrets** and add a new secret for the JSON key, such as `GOOGLE_SERVICE_ACCOUNT_KEY`.
1. Follow the Bitrise guide on [Connecting a Google Service Account](https://devcenter.bitrise.io/en/connectivity/connecting-a-google-service-account-to-bitrise.html) for more details.

### You Are Finished!

By completing the setup of your keystore, Apple service connection, iOS code signing, and optional Google service account you enable Bitrise to handle code signing and integration with necessary services, ensuring smooth builds and deployments for both iOS and Android.

Now (with all configurations, environment variables, and secrets in place) your Bitrise workflow is fully equipped to build and deploy Mendix Native apps in the cloud!
