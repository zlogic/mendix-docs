---
title: "Integrate Function Calling in your Mendix app"
url: /appstore/modules/genai/genai-howto-functioncalling/
linktitle: "Integrate Function Calling in your Mendix app"
weight: 30
description: "A tutorial guiding you through integrating and implementing function calling in your Mendix application for enhanced functionality."
---

## Introduction {#introduction}

This document guides you on how to use function calling in your smart app. For this, you can already use your existing app, or follow the steps from the [Build a Smart App from a Blank GenAI App](/appstore/modules/genai/using-genai/blank-app/) documentation, as demonstrated in this guide.

### Pre-requisites {#prerequisites}

Before diving into this guide, ensure you meet the following requirements:

- **An existing app**: To simplify your first use case, we start building from an already setup [Blank GenAI Starter App](https://marketplace.mendix.com/link/component/227934) as described in the [Build a Chatbot from Scratch Using the Blank GenAI App](/appstore/modules/genai/using-genai/blank-app/) tutorial. 

- **Installation** of the [GenAI For Mendix](https://marketplace.mendix.com/link/component/227931) bundle from the Mendix marketplace. If you start with the Blank GenAI App, this step can be skipped.

- **Intermediate knowledge of the Mendix platform**: Familiarity with Mendix Studio Pro, microflows, and modules.

- **Basic understanding of GenAI concepts**: Review the [Enrich Your Mendix App with GenAI Capabilities](/appstore/modules/genai/) page for foundational knowledge and familiarize yourself with the [concepts](/appstore/modules/genai/using-gen-ai/).

- **Understanding Function Calling and Prompt Engineering**: Learn about [Function Calling](/appstore/modules/genai/function-calling/) and [Prompt Engineering](/appstore/modules/genai/using-gen-ai/#prompt-engineering) for use within the Mendix ecosystem.

### Learning Goals {#learning-goals}

By following this tutorial, you will:

- Understand how to implement function calling within your Mendix application.

- Integrate GenAI capabilities to address specific business use cases.

## Function calling use case {#use-case}

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-functioncalling/structure_functioncalling.png" >}}

For this example, we'll implement two functions with the following purposes: 

1. Gathering the display name of the user when an email is requested in a chatbot, so that the information will automatically be filled for the end user, and
2. Extracting the bank holidays from the Netherlands using an API. For this example, a public API was taken from https://www.openholidaysapi.org/en/.

### Choosing the Infrastructure {#infrastructure}

Selecting the infrastructure for integrating GenAI into your Mendix application is the first step. Depending on your use case and preferences, you can choose from the following options:

* [Mendix Cloud GenAI Resource Packs](/appstore/modules/genai/MxGenAI/): The Mendix Cloud GenAI Connector is part of the [GenAI For Mendix](https://marketplace.mendix.com/link/component/227931) bundle on the marketplace, which lets you utilize Mendix Cloud GenAI Resource Packs directly within your Mendix application.

* [OpenAI](/appstore/modules/genai/openai/): The [OpenAI Connector](https://marketplace.mendix.com/link/component/220472) supports both OpenAI’s platform and Azure’s OpenAI service.

* [Amazon Bedrock](/appstore/modules/genai/bedrock/): The [Amazon Bedrock Connector](https://marketplace.mendix.com/link/component/215042) allows you to leverage Amazon Bedrock’s fully managed service to integrate foundation models from Amazon and leading AI providers. 

* Your Own Connector: Optionally, if you prefer a custom connector, you can integrate your chosen infrastructure. However, this document focuses on the Mendix Cloud GenAI, OpenAI and Amazon Bedrock connectors, as they offer comprehensive support and ease of use to get started.

Note: not all models support function calling. Make sure you set up your preferred GenAI provider in you Mendix app and have a model available that supports function calling. Mendix provides an [overview of models and their capabilities](https://docs.mendix.com/appstore/modules/genai/#models).

### Customizing microflows {#microflows}

To make the functions work, we need to create and adjust certain microflows as shown below. These microflows will handle the logic required for gathering the display name of the user and extracting the bank holidays from the Netherlands in 2025 using an API.

1. Locate the pre-built microflow `ChatContext_ChatWithHistory_ActionMicroflow` in **ConversationalUI > USE_ME > Conversational UI > Action microflow examples** and copy it into your `MyFirstBot` module.

2. Locate the `New Chat` action in the `ACT_FullScreenChat_Open` microflow. Inside this action, change the `Action microflow` input parameter to your new `MyFirstBot.ChatContext_ChatWithHistory_ActionMicroflow` from your `MyFirstBot` module.

In order to call a function, we need to create a microflow per function to extract the neccesary information.

#### Function: Extracting the user name {#function-username}

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-functioncalling/GetCurrentUserName_Function.jpg" >}}

Create new microflow with name `GetCurrentUserName_Function`. 

1. Start with the `Retrieve` action, where you can use the following modifications as an example: 
    * Source: `From database`
    * Entity: `Administration.Account`
    * Range: `First`
    * XPath constraint: `[id = $currentUser]`
    * Object name: `Account`

2. We include a decision where:
    * Caption: e.g. `Found?`
    * Decision Type: `Expression`
    * Expression: `$Account != empty`

    2.1. If the **decision** is `false`, an **End event** of type `String` is added where the **return value** can be set to something like `Mendix Administrator Chat User`.

    2.2. If the **decision** is `true`, an **End event** of type `String` is added where the **return value** is `$Account/FullName`.
    
#### Function: Getting bank holidays in the Netherlands 2025 {#function-bankholidays}

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-functioncalling/GetBankHolidays_Function.jpg" >}}

 For this example, we will call the new microflow `GetBankHolidays_Function`. 

 1. Start with the `Call REST service` action, where you can use the following modifications as an example: 
    General tab:
    * Location: `https://openholidaysapi.org/PublicHolidays?countryIsoCode=NL&validFrom=2025-01-01&validTo=2025-12-31&languageIsoCode=EN`
    * HTTP method: `GET`
    * Use timeout on request: `Yes`
    * Timeout (s): You can choose the value, here we set it to `300`
    * The rest can be set to default.
    Response tab:
    * Response handling: `Store in a string`
    * Store in variable: `Yes`
    * Variable name: `HolidayJSON`

2. Right click on the `Call REST` action and select `Set $HolidayJSON as return value`. 

### Calling the functions {#calling-the-functions}

Now that this part is settled, the following steps will focus exclusively on the `ChatContext_ChatWithHistory_ActionMicroflow` from your `MyFirstBot` module.

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-functioncalling/CallingFunctions_Microflow.jpg" >}}

As seen in the picture, there are two relevant steps that have to be done in order for the two functions to be called. 

#### Adding functions to the request {#add-to-request}

1. After the outgoing `Request found` equals `true` decision, add the `Tools: Add Function to Request` toolbox action for the first function with the following settings: 

    * Resquest: `$Request`
    * Tool name: `get-current-user-name`
    * Tool description: `This function has no input, and returns a string containing the name of the user that is using the chat. It can be used to generate texts on behalf of the user, for example the signature of an email, e.g. "Best regards, [user's name]"`
    * Function microflow: here we select the `GetCurrentUserName_Function` microflow which was created in the previous step. 
    * Use return value: No

2. Following this action, we continue with the second function by adding the `Tools: Add Function to Request` action with the following settings: 

    * Resquest: `$Request`
    * Tool name: `get-bank-holidays-2025`
    * Tool description: `This function has no input, and returns a JSON containing the bank holidays in the Netherlands for the year 2025.`
    * Function microflow: here we select the `GetBankHolidays_Function` microflow which was created in the previous step. 
    * Use return value: No

### Optional: Changing the system prompt {#edit-systemprompt}

Optionally, you can change the system prompt to give the model additional instructions, e.g. the tone of voice. Therefore, we follow a similar approached as described in the [Build a Chatbot from Scratch Using the Blank GenAI App](/appstore/modules/genai/using-genai/blank-app/#changing-system-prompt).

1. Open the copied `ACT_FullScreenChat_Open` microflow from your `MyFirstBot` module.
2. Locate the **New Chat** action.
3. Inside this action, find the `System prompt` parameter, which has by default an empty value.
4. Update the `System prompt` value to reflect your desired behavior. For example, *`Answer like a Gen Z person. Always keep your answers short.`*
5. Save the changes.

## Testing and Troubleshooting {#testing-troubleshooting}

Before proceeding with testing, ensure that you have completed the Mendix Cloud GenAI, OpenAI, or Bedrock configuration as outlined in the [Build a Chatbot from Scratch Using the Blank GenAI App](/appstore/modules/genai/using-genai/blank-app/) tutorial, particularly the [Infrastructure Configuration](/appstore/modules/genai/using-genai/blank-app/#config) section. 

To test the Chatbot, navigate to the **Home** icon to open the chatbot interface. Start interacting with your chatbot by typing in the chat box.
For example, you could enter `Write a message to my colleague Max asking about a meeting to disucss the content for our next GenAI how-to.` or `How many bank holidays do I have in December?`

Congratulations! Your chatbot is now ready to use.

If an error occurs, check the **Console** in Studio Pro for details to help resolve the issue.