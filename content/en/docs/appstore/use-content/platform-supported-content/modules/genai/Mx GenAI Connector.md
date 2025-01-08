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

