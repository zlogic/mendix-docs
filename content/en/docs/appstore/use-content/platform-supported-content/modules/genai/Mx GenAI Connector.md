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

## Operations

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/mxgenAI-connector/synthia-domain-model.png" >}}

Configuration keys are stored persistently after they are imported (either via the UI or the exposed microflow). The three different types of configurations reflect the use cases this service supports. The specific operations are described below.

To use the operations, a `SynthiaConnection` must always be passed that refers to a Configuration. Use the `Create Synthia Connection` toolbox action to create the object. A `KnowledgeBaseName` must only be passed for knowledge base operations.

### Chat Completions Operations

After following the general setup above, you are ready to use the microflows in the **USE_ME > ChatCompletions** folder in your logic. Currently, three microflows for chat completions are exposed as microflow actions under the **Synthia (Text & Files)** category in the **Toolbox**.

These microflows expect a `SynthiaConnection` object that refers to a `ConfigurationTextGeneration`. 

In chat completions, system prompts and user prompts are two key components that help guide the language model in generating relevant and contextually appropriate responses. For more information on prompt engineering, see the [Read More]{#readmore} section. Different exposed microflow activities may require different prompts and logic for how the prompts must be passed, as described in the following sections. For more information on message roles, see the [ENUM_MessageRole](/appstore/modules/genai/commons/#enum-messagerole) enumeration in the *GenAI Commons*.

All chat completion operations within the connector expect to *Retrieve and Generate* support [Function Calling](#function-calling), [Vision](#vision), and [Document Chat](#document-chat).

For more inspiration or guidance on how to use the above-mentioned microflows in your logic, Mendix recommends downloading our [GenAI Showcase App](https://marketplace.mendix.com/link/component/220475), which demonstrates a variety of examples.

#### Chat Completions (without History)

The microflow activity `Chat Completions (without history)` supports scenarios where there is no need to send a list of (historic) messages comprising the conversation so far as part of the request. The operation requires a specialized [Connection](/appstore/modules/genai/commons/#connection) of type SynthiaConnection and a `UserPrompt` as a string. Additional parameters, such as system prompt, can be passed via the optional [Request](/appstore/modules/genai/commons/#request) object.
Functionally, the prompt strings can be written in a specific way and can be tailored to get the desired result and behavior. For more information on prompt engineering, see the [Read More]({#readmore}) section.

Optionally, you can also use [Function Calling](#function-calling) by adding a [ToolCollection](/appstore/modules/genai/commons/#toolcollection) to the request or you can send [images](#vision) or [documents](#document-chat) along with the user prompt by passing a [FileCollection](/appstore/modules/genai/commons/#filecollection).

#### Chat Completions (with History)

The microflow activity `Chat completions (with history)` supports more complex use cases where a list of (historical) messages (for example, the conversation or context so far) is sent as part of the request to the LLM. The operation requires a specialized [Connection](/appstore/modules/genai/commons/#connection) of type SynthiaConnection, a [Request](/appstore/modules/genai/commons/#request) object containing messages, optional attributes, or an optional `ToolCollection`.

Optionally, you can use [Function Calling](#function-calling) by adding a [ToolCollection](/appstore/modules/genai/commons/#toolcollection) to the request or you can send [images](#vision) or [documents](#document-chat) along with the user prompt by passing a [FileCollection](/appstore/modules/genai/commons/#filecollection).

#### Chat Completions (Retrieve & Generate)

The microflow activity `Chat Completions Retrieve & Generate` simplifies `Retrieve and Generate` use cases without history. By providing a user prompt, the knowledge base is searched for similar knowledge chunks, which are then passed to the model. The model is instructed to base its response on the retrieved knowledge while referring to the source used to generate the response. This operation requires two specialized [Connection](/appstore/modules/genai/commons/#connection) of type SynthiaConnection, each linked to a `ConfigurationTextGeneration` and a `ConfigurationKnowledgeBase`, respectively.

Optionally, a [Request](/appstore/modules/genai/commons/#request) object can be provided to include additional parameters or to pass instructions via the `SystemPrompt`. Additionally, adding an extension to the `Request` through the `Configure Retrieve & Generate` action enables other filter options, making the `Request` object mandatory in such cases.

The returned `Response` includes [References](/appstore/modules/genai/commons/#reference) if the model used them to generate its response. In some cases, a knowledge chunk consists of two texts: one for the semantic search step and another for the generation step. For example, when solving a problem based on historical solutions, the semantic search identifies similar problems using their descriptions, while the generation step produces a solution based on the corresponding historical solutions. In those cases, you can add [MetaData](/appstore/modules/genai/commons/#chunkcollection-add-knowledgebasechunk) with the key `knowledge` to the chunks during the insertion stage, allowing the model to base its response on the specified metadata rather than the input text.

Additionally, to utilize the `Source` attribute of the references, you can include `MetaData` with the key `sourceUrl`. Finally, the `HumanReadableId` of a chunk is used to display the reference's title in the response.

#### Function Calling{#function-calling}

Function calling enables LLMs to connect with external tools to gather information, execute actions, convert natural language into structured data, and much more. Function calling thus enables the model to intelligently decide when to let the Mendix app call one or more predefined function microflows to gather additional information to include in the assistant's response.

The model does not call the function but rather returns a tool called JSON structure that is used to build the input of the function (or functions) so that they can be executed as part of the chat completions operation. Functions in Mendix are essentially microflows that can be registered within the request to the LLM​. The connector takes care of handling the tool call response and executing the function microflows until the API returns the assistant's final response.

Function microflows take a single input parameter of type string or no input parameter and must return a string. Currently, adding a [ToolChoice](/appstore/modules/genai/commons/#set-toolchoice) for function calling is not supported by the MxGenAIConnector.

{{% alert color="warning" %}}
Function calling is a highly effective capability and should be used with caution. Function microflows run in the context of the current user, without enforcing entity access. You can use `$currentUser` in XPath queries to ensure that you retrieve and return only information that the end-user is allowed to view; otherwise, confidential information may become visible to the current end-user in the assistant's response.

Mendix also strongly advises that you build user confirmation logic into function microflows that have a potential impact on the world on behalf of the end-user. Some examples of such microflows include sending an email, posting online, or making a purchase.
{{% /alert %}}

You can use function calling in all chat completions operations by adding a `ToolCollection` with a `Function` via the [Tools: Add Function to Request](/appstore/modules/genai/commons/#add-function-to-request) operation.
For more information, see [Function Calling](/appstore/modules/genai/function-calling/).

#### Vision{#vision}

Vision enables the model to interpret and analyze images, allowing them to answer questions and perform tasks related to visual content. This integration of computer vision and language processing enhances the model's comprehension and makes it valuable for tasks involving visual information. To ensure the vision inside the connector, an optional [FileCollection](/appstore/modules/genai/commons/#filecollection) containing one or multiple images must be sent with a single message.

For `Chat Completions without History`, `FileCollection` is an optional input parameter. For `Chat Completions with History`, `FileCollection` can optionally be added to individual user messages using [Chat: Add Message to Request](/appstore/modules/genai/commons/#chat-add-message-to-request).

In the entire conversation, you can pass up to 20 images that are smaller than 3.75 MB each and with a height and width of maximum 8000 pixels. The following types are accepted: PNG, JPEG, JPG , GIF, and WebP.

#### Document Chat{#document-chat}

Document chat enables the model to interpret and analyze documents, such as PDFs or Excel files, allowing them to answer questions and perform tasks related to the content. To use document chat, an optional [FileCollection](/appstore/modules/genai/commons/#filecollection) containing one or multiple documents must be sent along with a single message.

For `Chat Completions without History`, `FileCollection` is an optional input parameter. For `Chat Completions with History`, `FileCollection` can optionally be added to individual user messages using [Chat: Add Message to Request](/appstore/modules/genai/commons/#chat-add-message-to-request).

In the entire conversation, you can pass up to five documents that are smaller than 4.5 MB each. The following file types are accepted: PDF, CSV, DOC, DOCX, XLS, XLSX, HTML, TXT,and MD.

{{% alert color="info" %}}
When adding a document to the `FileCollection`, you can optionally use the `TextContent` parameter to pass the file name. Ensure the file name excludes its extension before passing it to the file collection.

Note that the model uses the file name when analyzing documents, which could make it vulnerable to prompt injection. Depending on your use case, you may choose to modify the string or not to pass it at all.
{{% /alert %}}
