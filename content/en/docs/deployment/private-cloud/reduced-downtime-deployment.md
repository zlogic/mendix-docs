---
title: "Reduced downtime deployment"
linktitle: "Reduced downtime deployment"
url: /developerportal/deploy/private-cloud-reduced-downtime/
description: "Describes how to reduce downtime when deploying apps in Private Cloud environments"
weight: 35
---
## Introduction

Kubernetes allows to update an app without downtime by [performing a rolling update](https://kubernetes.io/docs/tutorials/kubernetes-basics/update/update-intro/). Instead of stopping an app and then starting it with an updated version or configuration, Kubernetes can replace pods (replicas) one by one with an updated version. Existing pods will handle requests until the newer version is fully started.

Any changes in the [domain model](/refguide/domain-model/) need a database (schema) update; while Mendix is updating the schema, it's not possible to modify any persistent entities.

The Private Cloud Operator uses a [recreate](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#recreate-deployment) strategy by default: stop the current version (configuration) of an app, then start the new version.

Alternatively, the Private Cloud Operator can a `PreferRolling` strategy:

* the Operator will _try_ to perform a rolling update whenever possible;
* if the Operator detects that a database (schema) update is needed, it will switch to a Recreate strategy to perform a full restart.

If the new version of the app has model changes, deploying it will require a schema update.
In this case, the Private Cloud Operator will automatically stop all replicas of the app, causing downtime.

This feature works with Mendix for Private Cloud version 2.20 (and later).

### How a PreferRolling strategy works

When the Operator is configured to use a `PreferRolling` strategy, it will try to do an optimistic update and use a `Rolling` strategy.

If the app needs to perform a database (schema) update, it will signal the Operator that it's waiting for approval to perform the update.
The Operator will then switch the app to use a `Recreate` strategy (stop all current replicas and start an updated version) and wait until all replicas of the app are running the same MDA version.

Then, the Operator will let the app know it's safe to update the database schema, and approve the process.

As a `Rolling` strategy can run multiple versions of the app at the same time, requests from the browser need to be routed to a matching app version (an app that has the same microflow or nanoflow parameters).
The operator uses Kubernetes Service labels to perform an atomic switch, and instantly switch all clients to the updated version.
This will be done automatically once the number of updated replicas reaches a certain threshold (default: 50% of all replicas, specified in the [switchoverThreshold](#prefer-rolling-in-standalone) parameter).

## How to enable reduced deployment downtime

To enable the `PreferRolling` deployment strategy:

* The app needs to have 2 or more replicas specified in the `MendixApp` CR or Developer Portal.
* For Connected environments, the _Low downtime Deployment Strategy_ option needs to be enabled in the Cloud Portal.
* For Standalone environments, `deploymentStrategy` needs to be set to `PreferRolling` in the `MendixApp` CR.

If the app can be deployed without having to modify the database schema (model), it will be deployed using a Rolling deployment strategy.

For example, these changes can be done without downtime:

* Changing app constants, MxAdmin password or debugger settings
* Changing environment variables, Runtime or Java options
* Changing Runtime Metrics settings
* Upgrading the Mendix Operator version
* Rebuilding the same MDA version to get the latest CVE updates
* Changes in microflows or Java actions that don't affect the model
* Updating Java dependencies

Changes in the UI can be done without downtime, but as soon as the new version becomes available, all clients will be log in again:

* Page changes, including layout or CSS changes
* Changes in nanoflows or microflow parameters (if the microflow is used on a page)
* Changes in Javascript actions

These changes will be deployed with downtime, because the model needs to be updated:

* Adding Marketplace modules that have persistent entities
* Updating the object model in the app itself, or its Marketplace modules
* Updating to a newer Mendix version

<!-- TODO: document how to navigate and enable this in Portunus -->

#### Use PreferRolling strategy in Standalone environments {#prefer-rolling-in-standalone}

To enable reduced downtime deployments, add the `deploymetStrategy` section to your `MendixApp` CR

```yaml
apiVersion: privatecloud.mendix.com/v1alpha1
kind: MendixApp
metadata:
# ...
# omitted lines for brevity
# ...
spec:
  # ...
  # omitted lines for brevity
  # ...
  # Add or update this section:
  deploymentStrategy:
    type: PreferRolling
    switchoverThreshold: 50%
    rollingUpdate:
      maxSurge: 0
      maxUnavailable: 50%
```

For more information on the `MendixApp` CR, see [Editing CR](/developerportal/deploy/private-cloud-operator/#edit-cr) instructions.

If the `deploymentStrategy` is not specified, the Operator will use the `Recreate` strategy and perform a complete restart on any changes, causing downtime.
This follows how updates were processed by Mendix for Private Cloud versions before 2.19 and earlier.

You can specify the folliwing options:

* **type**: – specifies a type, `Recreate` (default) or `PreferRolling` (try to deploy without downtime when possible).
* **switchoverThreshold**: – specifies a threshold (percentage or absolute value) of updated, ready replicas when all clients should switch to the updated version.
    For example, setting this to `50%` will switch all clients to the updated app version once 50% of all replicas are running the updated version.
    If not specified, will use `50%` as the default value. This option is only used if the strategy **type** is set to `PreferRolling`.
* **rollingUpdate**: - specifies parameters for `Rolling` updates if the Operator is able to perform the update without performing a restart. These parameters are used as Kubernetes [rollingUpdate](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-update-deployment) parameters:
    * **maxSurge**: – specifies an absolute (or percentage) value how many additional replicas can be added during the deployment process. The default `0` value means that no additional replicas are added during the rollout process, and instead existing replicas are stopped (to avoid using additional cluster resources).
    * **maxUnavailable**: – specifies an absolute (or percentage) value how many replicas can be stopped (to be replaced with updated versions) during the rollout process. The default `50%` value means that half of the replicas would be stopped during the update process. Lowering this value slows down the rollout process, but in exchange ensures that less replicas are stopped during the update process.

## Preventing Kubernetes disruptions

Kubernetes can stop an app's pods if needs to stop a node (to scale down and consolidate apps to run on fewer nodes), or perform a node update (for example, install CVE patches on the host OS).

A [PodDisruptionBudget](https://kubernetes.io/docs/tasks/run-application/configure-pdb/) can be added to an app to ensure that Kubernetes only stops a limited number of an app's pods, and if necessary wait for replacement pods to become available.

To add a PodDisruptionBudget, create the following PodDisruptionBudget, replacing `<mendixapp-cr-name>` with your app's internal name (MendixApp CR name):

```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: <mendixapp-cr-name> # This should be updated to match the MenedixApp CR name
spec:
  maxUnavailable: 1 # Ensure that at most 1 replica is stopped by Kubernetes
  selector:
    matchLabels:
      privatecloud.mendix.com/app: <mendixapp-cr-name> # This should be updated to match the MenedixApp CR name
      privatecloud.mendix.com/component: mendix-app
```

## Known Limitations

* This feature is only supported by Mendix Operator version 2.20 (and later).
* If the new app version has are UI changes, all clients will be automatically logged out and will need to sign in into the app
* Deploying a new version of the app will cause downtime if there are changes in the domain model, or the Mendix version.
* If an app is based on Mendix 9.12 or a later version, a Rolling update will can run scheduled events on any replica. During an update, scheduled events might use a newer or older version of their associated microflows, which will be random. If there are major changes in a scheduled event microflow, consider temporarily disabling scheduled events during an update.

