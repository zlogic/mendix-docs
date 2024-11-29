---
title: "Entitlements"
url: /control-center/entitlements/
description: "Describes the Entitlements page in the Mendix Control Center."
weight: 30
beta: true
no_list: true 
---

{{% alert color="info" %}}
This feature is currently in beta. For more information, see [Beta Releases](/releasenotes/beta-features/).
{{% /alert %}}

## Introduction

The **Entitlements** page in Control Center is a self-service tool that displays the transactions using cloud credits. You can use the page to monitor your consumption of cloud credits.

{{< figure src="/attachments/control-center/apps/entitlements/entitlements.png" alt="entitlements page" class="no-border" >}}

## What Are Cloud Credits? {#cloud-credits}

{{% alert color="info" %}}
Mendix is working on changing cloud credits to cloud tokens. For more information about this change, see [Cloud Tokens FAQ](#cloud-tokens-faq).
{{% /alert %}}

Cloud credits are virtual credits that you can spend on the Mendix Platform to purchase [cloud resource packs](/developerportal/deploy/mendix-cloud-deploy/#resource-pack). If you want to top up the cloud credits, you can just purchase standard, premium, or premium plus cloud resource packs. Your purchase will be converted into cloud credits and you can then spend the cloud credits on any cloud resource pack available to you.

To use cloud credits, you need to enable self-service. If you want to enable self-service or have questions about cloud credits, contact your Customer Success Manager (CSM).

{{% alert color="info" %}}
From now on, you can only purchase and provision standard, premium, and premium plus cloud resource packs, not legacy resource packs. The cloud credits for legacy resource packs that you already purchased will be credited back to your account if you deprovision an environment.
{{% /alert %}}

## Cloud Resource Packs

{{% alert color="info" %}}
For the technical details of each cloud resource pack, see the [Cloud Resource Packs](/developerportal/deploy/mendix-cloud-deploy/#resource-pack) section in *Mendix Cloud*.
{{% /alert %}}

The tables below show how many cloud credits each cloud resource pack costs:

| Standard Resource Packs    | Cloud Credits |
| ------------------------------ | ------------- |
| XS21                           | 1             |
| S21                            | 2             |
| M21                            | 4             |
| L21                            | 8             |
| XL21                           | 16            |
| XXL21                          | 32            |
| XXXL21                         | 64            |
| XXXXL21                        | 128           |

|Premium Resource Packs                  | Cloud Credits |
| ------------------------------ | ------------- |
| S21 Premium                    | 3             |
| M21 Premium                    | 6             |
| L21 Premium                    | 12            |
| XL21 Premium                   | 24            |
| XXL21 Premium                  | 48            |
| XXXL21 Premium                 | 96            |
| XXXXL21 Premium                | 192           |
| XL21 Premium Plus              | 40            |
| XXL21 Premium Plus             | 80            |
| XXXL21 Premium Plus            | 160           |
| XXXXL21 Premium Plus           | 320           |

| Legacy Resource Packs | Cloud Credits |
| ------------------------------ | ------------- |
| XS20    | 1             |
| S20     | 2             |
| M20     | 4             |
| L20     | 8             |
| XL20    | 16            |
| XXL20   | 32            |
| Strato  | 1.2           |
| Meso    | 4.67          |
| Iono    | 6.67          |
| Magneto | 14.67         |
| S       | 0.8           |
| M       | 1.6           |
| L       | 3.73          |
| XL      | 7.33          |
| XXL     | 16.67         |
| XXXL    | 64            |

## Cloud Tokens FAQ {#cloud-tokens-faq}

### What Are Mendix Cloud Tokens?

Mendix cloud tokens are annual capacity-based virtual credits that allow you to provision and allocate any Mendix cloud resource pack for your Mendix Cloud deployments.

Mendix cloud tokens are the successor to cloud credits.

### I Am Already Working with Cloud Credits. What Changes Will I See When Cloud Tokens Are Introduced?

You will notice the following changes:

* Name change: cloud tokens are the successor to cloud credits and completely replace this concept. Once introduced, cloud tokens will replace cloud credits throughout the Mendix Platform. This name change will primarily affect the **Entitlements** page and the **Deployed Apps** page in Control Center.
* Value adjustment: Mendix cloud resource packs are valued differently with cloud tokens compared to cloud credits. You will see this change on the **Entitlements** page in Control Center. Once cloud tokens are introduced, your existing transactions and cloud tokens will be automatically adjusted.
* Direct ordering: you will be able to order Mendix cloud tokens directly from the Mendix pricelist. You can use your available cloud tokens to provision any cloud resource pack for your apps.

### Is the Value of Cloud Credits and Cloud Tokens the Same?

No, one cloud credit is equivalent to ten cloud tokens. For example, the smallest cloud resource pack, XS standard resource pack, which was valued at one cloud credit, is now equivalent to ten cloud tokens.

### When Can I Purchase Mendix Cloud Tokens as a Product?

Mendix cloud tokens will be available as a product on the Mendix pricelist, starting in early 2025.

### Where Can I Ask Any Further Questions Regarding Mendix Cloud Tokens?

For any questions, contact your Mendix Customer Success Manager. If you experience any issues, create a support ticket with Mendix Support.
