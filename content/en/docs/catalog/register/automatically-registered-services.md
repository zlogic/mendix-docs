---
title: "Automatically registered services"
linktitle: "Automatically registered services"
url: /catalog/register/automatically-registered-services
description: "Provides explanation of automatic registration of services in Catalog"
weight: 50
aliases:
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details. 
---

## Introduction

The Catalog automatically registers published and consumed services when your application is deployed through the Mendix Cloud. Each deployment provides Catalog with the metadata of your services allowing us to not only register all the services published by an application, but also keep it up to date when any changes are made. This makes them easily discoverable within your organization. Supported publishied service types that are automatically registered are OData, REST, Web Services and Business Events. For consumption, only consumed OData services are automatically registered.

## Prerequisites

For OData your app needs to be on Mendix Studio Pro version 8.14.0 or above.

For REST, Web Services and Business Events your app needs to be on Mendix Studio Pro version 10.0 or above.

If your services are not hosted on Mendix Cloud, see [Register Resources](/catalog/register/) to explore alternative methods to register them.

## Who Can Discover My Services?

Once your services are registered in the Catalog, they become discoverable to other Mendix platform users within your organization.

You can manage discoverability on a per-service basis, either from the curation pages or the service detail page. For more information, see the [Discoverable and Validated](/catalog/manage/curate/#discoverability) section of *Curate Registered Assets*.


## Read More

To dive deeper into how you can make the most out of the Catalog and its capabilities, check out our comprehensive [Getting Started Guide](/catalog/get-started/).
