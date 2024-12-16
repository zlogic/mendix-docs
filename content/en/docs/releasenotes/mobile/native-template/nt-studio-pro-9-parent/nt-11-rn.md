---
title: "Native Template 11"
url: /releasenotes/mobile/nt-11-rn/
weight: 5
description: "Native Template 11 release notes."
---

## 11.0.0 {#1000}

**Release date: December 17, 2024**

### Breaking Changes

#### JSC and Hermes Support

* We disabled JavascriptCore(JSC) entirely and now only support Hermes.

#### Important Notes

* Projects created in Studio Pro 10.18 and above will automatically use Hermes without any additional configuration.
* For projects upgrading from Mendix versions below 10.18 to 10.18 and above, follow the steps in [Upgrade Instructions](#upgrade-instructions) below to migrate your project.
* Even if your project is already using Hermes, the update is still necessary.
