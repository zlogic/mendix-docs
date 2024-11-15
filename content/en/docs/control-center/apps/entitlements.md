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

{{< figure src="/attachments/control-center/entitlements/entitlements.png" alt="entitlements page" class="no-border" >}}

## What Are Cloud Credits? {#cloud-credits}

{{% alert color="info" %}}
From December 5, 2024, cloud credits will be changed to cloud tokens. For more information about this change, see [Cloud Tokens FAQ](#faq).
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

### What Changes Will I See When Mendix Cloud Tokens Are Introduced?

You will notice the following changes:

* Cloud credits will be changed to cloud tokens on the **Entitlements** page and the **Deployed Apps** page in Control Center.
* Your cloud credit balance and transactions will be multiplied by 10 to reflect their value in cloud tokens.
* You will be able to order Mendix cloud tokens directly from the Mendix Pricelist.

### Has the Value of My Resources Changed, Either My Entitlements or the Cloud Resources that I Consumed?

No. Your cloud credits are just renamed to cloud tokens and all values have been multiplied by a factor of 10. As this is done both for your total entitlements and for the value of the cloud resources you consumed, nothing effectively changed. For example, if you were entitled to 30 cloud credits before and you consumed 20, you will now be entitled to 300 cloud tokens and will have consumed 200 cloud tokens. The actual total cloud resources that you can use remains the same.

### Why Are My Cloud Token Balance and Transaction Values Ten Times the Values of My Cloud Credits?

Mendix sets the value of each cloud token to be 10 times that of a cloud credit to allow for smaller-priced cloud products. This greater precision allows Mendix to later include lower-priced products in the cloud token system while eliminating the need for decimal pricing for cloud resources.

### Why Are Cloud Tokens Measured in Integer Values Only?

Mendix strives to make things as simple and flexible as possible, not only with the technical ecosystem, but also with the purchasing model. Mendix decided that having only integer (whole numbers) values for the cloud resources should be part of that, as it makes values clearer and counting much easier for everyone.

### How Did Mendix Round Cloud Credit Values with Two Significant Decimals?

In the conversion from cloud credits to cloud tokens, Mendix chose a factor of ten to let the minimal token value reflect and fit the value of the smallest cloud resources Mendix offers. This meant that balances and transactions with two significant decimals had to be rounded. Mendix applied the standard banking rounding rules for doing that.

### When Can I Purchase Mendix Cloud Tokens as a Product?

Mendix introduces this product at the start of 2025.

### What Cloud Products Can I Purchase Using Mendix Cloud Tokens?

At the introduction of cloud tokens, you can purchase cloud resource packs. In the future, more cloud products will be added.

### Where Can I Ask Any Further Questions Regarding Mendix cloud Tokens?

You can reach out to your Mendix Customer Success Manager if you have any questions about Mendix cloud tokens.
