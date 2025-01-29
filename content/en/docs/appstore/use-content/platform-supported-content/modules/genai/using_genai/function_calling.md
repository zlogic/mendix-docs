---
title: "Integrate Function Calling in your Mendix app"
url: /appstore/modules/genai/genai-howto-functioncalling/
linktitle: "How to integrate Function Calling in your Mendix app"
weight: 10
description: "A tutorial guiding you through integrating and implementing function calling in your Mendix application for enhanced functionality."
---

## Introduction
This document guides you on how to use function calling in your smart app. For this, you can already use your existing app, or follow the steps from the [Build a Smart App from a Blank GenAI App](/appstore/modules/genai/using-genai/blank-app/) documentation, as demonstrated in this guide.

### Pre-requisites

Before diving into this guide, ensure you meet the following requirements:

- **An existing app**: In case you do not have anything existing yet, feel free to check the [Build a Chatbot from Scratch Using the Blank GenAI App](/appstore/modules/genai/using-genai/blank-app/) documentation, as outlined in this tutorial. 

- **Installation** of the [GenAI For Mendix](https://marketplace.mendix.com/link/component/227931) bundle.

- **Intermediate knowledge of the Mendix platform**: Familiarity with Mendix Studio Pro, microflows, and modules.

- **Basic understanding of GenAI concepts**: Review the [Enrich Your Mendix App with GenAI Capabilities](/appstore/modules/genai/) page for foundational knowledge and familiarized yourself with the [concepts](/appstore/modules/genai/using-gen-ai/).

- **Understanding Function Calling and Prompt Engineering**: Learn about [Function Calling](/appstore/modules/genai/function-calling/) and [Prompt Engineering](/appstore/modules/genai/using-gen-ai/#prompt-engineering) for use within the Mendix ecosystem.


### Learning Goals

By following this tutorial, you will:

- Understand how to implement function calling within your Mendix application.

- Integrate GenAI capabilities to address specific business use cases.

## Function Calling 

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-functioncalling/structure_functioncalling.png" >}}

As mentioned above, to simplify your first use case, we start building from an already setup [Blank GenAI App Template](https://marketplace.mendix.com/link/component/227934) as described in the [Build a Chatbot from Scratch Using the Blank GenAI App](/appstore/modules/genai/using-genai/blank-app/) tutorial. For illustration, the two functions/tools will have the following purposes: (1) gathering the display name of the user when an email is requested in a chatbot, so that the information will automatically be filled for the end user, and (2) extracting the bank holidays from the Netherlands using an API.

### Choosing the Infrastructure

Selecting the infrastructure for integrating GenAI into your Mendix application is the first step. Depending on your use case and preferences, you can choose from the following options:

* [Mendix Cloud GenAI Resources Packs](/appstore/modules/genai/MxGenAI/): Part of the [GenAI For Mendix](https://marketplace.mendix.com/link/component/227931), integrates LLMs by dragging and dropping common operations from its toolbox in Studio Pro.

* [OpenAI](/appstore/modules/genai/openai/): The [OpenAI Connector](https://marketplace.mendix.com/link/component/220472) supports both OpenAI’s platform and Azure’s OpenAI service.

{{% alert color="info" %}}
To start, you can sign up for a free trial with OpenAI and receive credits valid for three months from the account creation date. For more details, see [OpenAI API reference](https://platform.openai.com/docs/api-reference/authentication).
{{% /alert %}}

* [Amazon Bedrock](/appstore/modules/genai/bedrock/): The [Bedrock Connector](https://marketplace.mendix.com/link/component/215042) allows you to leverage Amazon Bedrock’s fully managed service to integrate foundation models from Amazon and leading AI providers. 

* Your Own Connector: Optionally, if you prefer a custom connector, you can integrate your chosen infrastructure. However, this document focuses on the OpenAI and Bedrock connectors, as they offer comprehensive support and ease of use to get started.

### Customizing microflows

To make the functions work, we need to create and adjust certain microflows as shown below. These microflows will handle the logic required for gathering the display name of the user and extracting the bank holidays from the Netherlands using an API.

1. Locate the pre-built microflow `ChatContext_ChatWithHistory_ActionMicroflow` in **ConversationalUI > USE_ME > Conversational UI > Action microflow examples**. Right-click on the microflow and select **Include in project** to copy it into your `MyFirstBot` module.

2. Locate the `ChatContext` action in the `ACT_FullScreenChat_Open` microflow. Inside this action, change the `Action microflow` to `ChatContext_ChatWithHistory_ActionMicroflow` from your `MyFirstBot` module.

In order to call a function, we need to create a microflow per function to extract the neccesary information.

#### Function: Extracting display name

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-functioncalling/GetCurrentUserName_Function.jpg" >}}

For this example, we will call the new microflow `GetCurrentUserName_Function`. 

1. Start with the `Retrieve Objects` action, where you can use the following modifications as an example: 
    * Source: `From database`
    * Entity: `Administration.Account`
    * XPath constraint: `[id = $currentUser]`
    * Range: `First`
    * Object name: `Account`

2. We include a decision where:
    * Caption: e.g. `Found?`
    * Decision Type: `Expression`
    * Expression: `$Account != empty`

    2.1. If the **decision** is `false`, an **End event** is added with the **Microflow return value** can be set to something like `Mendix Administrator Chat User` with a **Return variable name** as `UserName`. 

    2.2. If the **decision** is `true`, an **End event** is added with the **Microflow return value** is `$Account/FullName` with a **Return variable name** as `UserName`. 
    
#### Function: Extracting display name

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-functioncalling/GetBankHolidays_Function.jpg" >}}

 For this example, we will call the new microflow `GetBankHolidays_Function`. 

 1. Start with the `Call REST Service` action, where you can use the following modifications as an example: 
    * Location: `https://openholidaysapi.org/PublicHolidays?countryIsoCode=NL&validFrom=2025-01-01&validTo=2025-12-31&languageIsoCode=EN`
    * HTTP method: `GET`
    * use timeout on request: `Yes`
    * Timeout (s): You can choose the value, here we set it to `300`
    * The rest can be set to default.

2. The next step is an **End event** with the **Microflow return value** of `$HolidaysJSON` with a **Return variable name** as `Variable`. 

### Calling the functions

Now that this part is settled, the following steps will focus exclusively on the `ChatContext_ChatWithHistory_ActionMicroflow` from your `MyFirstBot` module.

{{< figure src="/attachments/appstore/platform-supported-content/modules/genai/genai-howto-functioncalling/CallingFunctions_Microflow.jpg" >}}

As seen in the picture, there are two relevant steps that have to be done in order for the two functions to be called. 

#### Addition of Tools action

1. After the `true` `Request found` decision, add the `Tools: Add Function to Request` action for the first function with the following settings: 

    * Resquest: `$Request`
    * Tool name: `get-current-user-name`
    * Tool description: `This function has no input, and returns a string containing the name of the user that is using the chat. It can be used to generate texts on behalf of the user, for example the signature of an email, e.g. "Best regards, [user's name]"`
    * Function microflow: here we call the `GetCurrentUserName_Function` microflow previously done. 
    * Use return value: No

2. Following this action, we continue with the second function by adding the `Tools: Add Function to Request` action with the following settings: 

    * Resquest: `$Request`
    * Tool name: `get-bank-holidays-2025`
    * Tool description: `This function has no input, and returns a JSON containing the bank holidays in The Netherlands for the year 2025.`
    * Function microflow: here we call the `GetBankHolidays_Function` microflow previously done. 
    * Use return value: No

### Changing the system prompt

It is important to remember that a function will only be called correctly if the LLM knows when and which function to call. Therefore, we follow a similar approached as described in the [Build a Chatbot from Scratch Using the Blank GenAI App](/appstore/modules/genai/using-genai/blank-app/#changing-system-prompt).

1. Open the copied `ACT_FullScreenChat_Open` microflow from your `MyFirstBot` module.
2. Locate the **ChatContext** action.
3. Inside this action, find the `System prompt` parameter, which has default an empty value.
4. Update the `System prompt` value to reflect your desired behavior. For example, *`You are an assistant that supports users in a company with various requests. When a user asks you to write an email, ensure all necessary information is provided. If any information is missing, ask the user for more details. Use the function get-current-user-name to retrieve the user's display name for the email signature. If the user asks for bank holidays in the Netherlands for the year 2025, use the function get-bank-holidays-2025 to retrieve this information. For any other requests, respond appropriately based on the context and provide helpful information. Always keep your answers short.`*
5. Save the changes.

## Testing and Troubleshooting

Before proceeding with testing, ensure that you have completed the Mendix Cloud GenAI, OpenAI, or Bedrock configuration as outlined in the [Build a Chatbot from Scratch Using the Blank GenAI App](/appstore/modules/genai/using-genai/blank-app/) tutorial, particularly the [Infrastructure Configuration](/appstore/modules/genai/using-genai/blank-app/#config) section. 

To test the Chatbot, navigate to the **Home** icon to open the chatbot interface. Start interacting with your chatbot by typing in the chat box.

Congratulations! Your chatbot is now ready to use.

If an error occurs, check the **Mendix Console** in Studio Pro for details to help resolve the issue.