---
title: "Mx GenAI Connector"
url: /appstore/modules/genai/MxGenAI/
linktitle: "Mx GenAI"
description: "Describes the configuration and usage of the MxGenAI Connector, which allows you to utilize Mendix Cloud GenAI Resource Packs directly within your Mendix application."
weight: 60
---

## Introduction

The [MxGenAIConnector](Marketplace link) lets you utilize Mendix Cloud GenAI resource packs directly within your Mendix application. It allows you to integrate generative AI by dragging and dropping common operations from its toolbox.

### Typical Use Cases

The MxGenAIConnector is commonly used for text generation, embeddings, and knowledge bases. These use cases are described in more detail below:

#### Text Generation

* Develop interactive AI chatbots and virtual assistants that can carry out conversations naturally and engagingly.
* Use large language models (LLMs) like Anthropic Claude 3.5 for text comprehension and analysis use cases such as summarization, synthesis, and answering questions about large amounts of text.
* By using text generation models, you can build applications with features such as:

    * Draft documents
    * Write computer code
    * Answer questions about a knowledge base
    * Analyze texts
    * Give the software a natural language interface
    * Tutor in a range of subjects
    * Translate languages
    * Simulate characters for games
    * Image to text

#### Knowledge Base

The module enables tailoring generated responses to specific contexts by grounding them in data from a knowledge base. This allows for the secure use of private company data or other non-public information when interacting with GenAI models within the Mendix app. It provides a low-code solution to store discrete data (commonly called chunks) in the knowledge base and retrieves relevant information for end-user actions or application processes.

Knowledge bases are often used for:

1. [Retrieval Augmented Generation (RAG)](https://docs.mendix.com/appstore/modules/genai/rag/) retrieves relevant knowledge from the knowledge base, incorporates it into a prompt, and sends it to the model to generate a response.
2. Semantic search enables advanced search capabilities by considering the semantic meaning of the text, going beyond exact and approximate matching. It allows the knowledge base to be searched for similar chunks effectively.

#### Embeddings

Convert strings into vector embeddings for various purposes based on the relatedness of texts.

Embeddings are commonly used for the following:

* Search 
* Clustering 
* Recommendations 
* Anomaly detection 
* Diversity measurement 
* Classification 

Combine embeddings with text generation capabilities and leverage specific sources of information to create a smart chat functionality tailored to your knowledge base.

{{% alert color="info" %}}
MxGenAIConnector module generates embeddings internally when interacting with the knowledge base. Pure embedding operations are only required if additional processes, such as using the generated vectors instead of text, are needed. For example, a similar search algorithm could use vector distances to calculate relatedness.
{{% /alert %}}

### Features

In the current version, Mendix supports text generation (including function/tool calling, chat with images, and chat with documents), vector embedding generation, knowledge base storage, and retrieval.

### Prerequisites

To use this connector, you need configuration keys to authenticate to the Mendix Cloud GenAI services. You can generate keys in the developer portal or ask someone with access to either generate them for you or be added to the team to generate keys yourself. For questions, reach out to `david.hartveld@mendix.com` for details.

### Dependencies {#dependencies}

* Mendix Studio Pro version [9.24.2](/releasenotes/studio-pro/9.24/#9242) or above
* [GenAI Commons](https://marketplace.mendix.com/link/component/227933)
* [Encryption](https://marketplace.mendix.com/link/component/1011)
* [Community Commons](https://marketplace.mendix.com/link/component/170)

## Installation

Add the [Dependencies](#dependencies) listed above from the Marketplace. To import the [MxGenAIConnector](marketplace link) into your app, follow the instructions in the [Use Marketplace Content](/appstore/use-content/).

## Configuration

After installing the MxGenAIConnector, you can find it in the **App Explorer** under the **Add-ons** section. The connector includes a domain model and several activities to help integrate your app with the Mendix Cloud GenAI service. To implement the connector, simply use its actions in a microflow. You can find the Mendix GenAI actions in the microflow toolbox. Note that the module is protected, meaning it cannot be modified and the microflow logic is not visible. For details about each exposed operation, see the [Operations](#operations) section below or refer to the documentation provided within the module. For more information on Add-on modules, see [Consuming Add-on Modules and Solutions](/refguide/consume-add-on-modules-and-solutions/).

Follow the steps below to get started:

* Make sure to configure the [Encryption module](/appstore/modules/encryption/#configuration) before you connect your app to Mendix Cloud GenAI.
* Add the module role `MxGenAIConnector.Administrator` to your Administrator **User roles** in the **Security** settings of your app. 
* Add the `NAV_ConfigurationOverview_Open` microflow (**USE_ME** > **Configuration**) to your **Navigation** or register your key using the `Configuration_RegisterByString` microflow.
* Complete the runtime setup of Mendix Cloud GenAI configuration by navigating to the page through the microflow mentioned above. Import a key generated in the portal or provided to you and click **Test Key** to validate its functionality.
