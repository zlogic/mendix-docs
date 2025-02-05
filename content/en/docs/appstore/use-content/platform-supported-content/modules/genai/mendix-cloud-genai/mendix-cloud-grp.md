---
title: "Mendix Cloud GenAI Resource Packs"
url: /appstore/modules/genai/mx-cloud-genai/resource-packs
linktitle: "Mendix Cloud GenAI Resource Packs"
description: "Explains what the Mendix Cloud GenAI Resource Packs is including the capabilities, limitations, and FAQ."
weight: 10
---

## Introduction

**Mendix Cloud GenAI Resource Packs** provide turn-key access to Generative AI technology, delivered through Mendix Cloud:

* **Model Resource Packs** provide customers with large language model capacity. Each resource pack comes with an allocation of input/output tokens for Anthropic's Claude and Cohere's Embed. We will introduce support for additional models in the future.
* **Knowledge Base Resource Packs** give customers an OpenSearch-based vector database to support Retrieval-Augmented Generation (RAG), Semantic Search and other Generative AI use cases.

Developers can use the Mendix Portal to manage their Mendix Cloud GenAI resources and easily integrate the models & knowledge base capabilities into their Mendix apps with the Mendix Cloud GenAI Connector. Optimized for high performance and low latency to ensure efficient processing of AI tasks, Mendix Cloud GenAI Resource Packs are the easiest & fastest way to deliver end-to-end Generative AI solutions, on a single platform.


### Limited Availability

Mendix Cloud GenAI Resource Packs are currently available under Limited Availability. Mendix is working with early adopters (customers, partners, ISVs) to enable to deliver project successes. Mendix currently evaluates use cases and selectively provides access to GenAI Resource Packs. Reach out to genai-resource-packs@mendix.com to learn more about the conditions for access to GenAI Resource Packs under Limited Availability until the General Availability release.

## Models

Mendix Cloud Model Resource Packs provide customers with a monthly quota of input and output tokens for Anthropic's Claude and Cohere's Embed models. This allows customers to implement typical Generative AI use cases like 

### Supported models

Mendix Cloud provides access to the following models:

* Anthropic Claude v3.5 Sonnet v1
* Cohere Embed v3 (English & Multilingual options)

The models are made available through the Mendix Cloud, leveraging AWS's highly secure Amazon Bedrock multi-tenant architecture, which employs advanced logical isolation techniques that effectively segregate customer data, requests, and responses, ensuring a level of data protection that aligns with global security compliance requirements. Customer prompts or requests and the corresponding answers or responses are not stored, nor are they used for model training. Your data remains your data.

Customers looking to leverage other models in addition to the above can also take advantage of Mendix' [(Azure) OpenAI Connector](/appstore/modules/genai/reference-guide/external-connectors/openai/) and Amazon [Bedrock Connector](/appstore/modules/genai/reference-guide/external-connectors/bedrock/) to integrate numerous other models in their apps.

## Knowledge Bases

Mendix Cloud Knowledge Base Resource Packs entitle customers to an elastic, logically isolated vector database, to use for standard Generative AI architectural patterns like Retrieval-Augmented Generation (RAG), for semantic similarity search or other Generative AI use cases. The Knowledge Bases on Mendix Cloud are based on AWS's highly secure Amazon Bedrock Knowledge Bases capability combined with AWS' OpenSearch Serverless database - the de facto standard infrastructure for Generative AI Knowledge Bases on AWS, guaranteeing fast & accurate information retrieval.

Knowledge bases enable you to bring your own data for RAG, semantic similarity search & other generative AI use cases:

* Make your app's data available through integration
* Connect to third-party information sources
* Manage knowledge base content & add metadata labels

Knowledge Bases are based on elastically scaling, serverless OpenSearch vector databases, to ensure high performance under load. The database is set up as a highly available cluster to ensure business continuity. Customer data is stored in logical isolation from other customers and is not used for model training, to guarantee data security & privacy in compliances with industry standards.

## Mendix Portal

The Mendix Portal allows easy access to manage the resources, through the GenAI Resources section in the portal.

* Get insight into consumption of input/output tokens against entitlements for Models
* Manage content for Knowledge Bases
* Manage team access to all resources
* Create & manage connection keys to connect your apps with all resources
* Track activity logs for team access & connection key management

## Mendix Cloud GenAI Connector

The [Mendix Cloud GenAI connector](/appstore/modules/genai/mx-cloud-genai/MxGenAI/) lets you utilize Mendix Cloud GenAI resource packs directly within your Mendix application. It allows you to integrate generative AI by dragging and dropping common operations from its toolbox.

## Regional availability

Mendix Cloud GenAI Resource Packs are available in the following regions of Mendix Cloud:

* Europe (Frankfurt) - eu-central-1

## FAQ

### What happens with data processed by Mendix Cloud GenAI services?

For the Mendix Cloud GenAI Model Resources with Anthropic's Claude and Cohere's Embed, we do not store any of the requests (prompts) & responses (answers, embeddings) sent to & from the service. Neither do our partners Amazon, Anthropic & Cohere. Your data is **not** used for training.

Data stored in the GenAI Knowledge Base Resources is stored in a logically isolated database only accessible by you, the customer, through the Keys you can create yourself in the Portal.

### How is data sent to the Mendix Cloud GenAI service stored & used?
The requests (prompts) sent to & responses (answers, embeddings) sent from the Models are not stored anywhere and are **not** used for training purposes. Only metadata about requests like token input/output counts are collected for logging, metrics & monitoring, metering & billing, product improvement and maintenance purposes.

Data sent to the Knowledge Base (vectors, chunks) is stored in a logically isolated, fully secure vector database in an industry-standard way. The data is **not** used by Mendix and exclusively available to you, the customer. Your data is **not** used for training purposes. Only metadata about use of the Knowledge Base is stored & used for logging, metrics & monitoring, metering & billing, product improvement and maintenance purposes.

### See also

* [Enrich your Mendix app with GenAI capabilities](/appstore/modules/genai/)
* [Build your first Mendix app with GenAI](/appstore/modules/genai/using-genai/starter-template)
