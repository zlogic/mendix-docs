---
title: "Online Synchronization Mode Beta"
url: /refguide/mobile/building-efficient-mobile-apps/offlinefirst-data/online-sync-mode/
no_list: false
description_list: true 
weight: 20
aliases:
    - /refguide/mobile/using-mobile-capabilities/offlinefirst-data/online-sync-mode/
---

## Introduction
In Mendix 10.19 the "online" synchronization mode has been introduced. This synchronization mode allows entities to be used in offline navigation profiles without the need for data synchronization for those entities. When online entities are used in offline pages, then they require a connection to the server before the data can be shown. This enables apps to become just partially offline instead of requiring being fully offline.

{{% alert color="warning" %}}
This feature is currently in Beta. It needs to be explicitly enabled (see below). This feature may change in the future and may not yet work completely as expected.
{{% /alert %}}

Creating offline-first apps enhances user experience by enabling mobile app usage without connectivity. However, offline-first apps often demand more effort to design and maintain due to the complexity of synchronization. At the same time, not all parts of an application benefit equally from offline accessibility. By incorporating online data in offline-first apps, you can define which parts of your application can function offline and which require an online connection. This approach can streamline development while maintaining a high user experience and essential offline capabilities.


## How to start using Online synchronization mode?
### Enabling the feature
It requires to be explicitly enabled via the "Edit" menu -> "Preferences" item -> "New features" tab -> "Offline" section.

### Configuring entities to have Online synchronization mode
Go to the "Navigation" tree item in your App section of the "App Explorer" in Studio Pro and go to the offline navigation profile on which you want to enable the online synchronization mode. This can be the "Native mobile (tablet & phone)", or the "Response web offline" profile. In that tab, click "Configure synchronization". When the feature is enabled, the Synchronization mode "Online" is available in the drop-down next to the entity in the table.

## Limitations
### Generalization/specialization
Online entities can specialize from Offline entities and vice versa. However, when there are different synchronization modes in the generalization/specialization hierarchy, then queries on the generalization entity are prohibited (as such queries will not only select data from the generalization, but also from the specializations).

### Association between offline and online entities
Associations from Offline Entities to Online Entities are allowed. Associations from Online Entities to Offline Entities are prohibited.

### Microflows
Using microflows on pages used from an offline navigation profile is allowed. However, calling a microflow from a nanoflow with an online entity as parameter or return value is prohibited. This is a temporary limitation which we expect to lift soon.

## Best Practice
Check for connection before calling the server
Offline apps may not always have connections to the server. Therefore, the suggested approach to open a page with online data is to check the connection first in a nanoflow. When there is no connection, you can model the app how to behave (present a dialog with retry option or navigate to a different page for example).
