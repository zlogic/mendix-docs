---
title: "Native Template 11"
url: /releasenotes/mobile/nt-11-rn/
weight: 5
description: "Native Template 11 release notes."
---

## 11.0.0 {#1000}

**Release date: December 17, 2024**

### Breaking Changes

#### JSC and Hermes Support {#jsc-hermes}

* We disabled JavaScriptCore(JSC) entirely and now only support Hermes.

#### Important Notes

* Apps created in Studio Pro 10.18 and above will automatically use Hermes without any additional configuration.
* For projects upgrading to 10.18 and above, follow the steps in [Upgrade Instructions](#upgrade-instructions) to migrate your app.
* Even if your project is already using Hermes, the update is still required.
