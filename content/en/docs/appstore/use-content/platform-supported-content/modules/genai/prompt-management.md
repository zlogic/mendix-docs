---
title: "Prompt Management"
url: /appstore/modules/genai/conversational-ui/prompt-management
linktitle: "Prompt Management"
weight: 20
description: "Describes the Prompt Management functionality that assists developers and data scientists in implementing promts in their GenAI Mendix applications use cases."
---

## Introduction {#introduction}

Prompt management allows users to develop, test and optimize their GenAI use cases by creating effective prompts to interact with large language models (LLM). 
Using the Conversational UI module (available as part of [GenAI for Mendix](https://marketplace.mendix.com/link/component/227931)), you can use the prompt management interface in your app to define prompts at runtime and manage multiple versions over the course of time. It also supports defining variables that serve as placeholders for data from the app session context which are replaced by actual values when the end user interacts with the app. The module contains the necessary data model, pages and snippets to include a prompt management interface to your app and get started.

### Typical Use Cases {#use-cases}

Typical use cases for prompt management include the following:

* The app includes one or more chat completions interactions with an LLM. 
* The prompts for the LLM interaction need to be updated or improved without changing the code of the LLM interaction. This enables people outside of the development team to change prompts (e.g., data scientists).
* The use case benefits from rapid iterations on prompts, models and variable placeholders in a playground set-up, separately from app logic.

### Features {#features}

The Prompt Management functionality provides the following:

* UI components and a data structure to manage, store and rapidly iterate on prompt versions at runtime: no app deployment needed to change the prompt.
* Support for single-call and conversational prompts.
* Include placeholders in prompts: the values will be populated in the running app based on a user/context object.
* Logic to define and execute tests, one-by-one or in bulk and compare the results.
* Export/import functionality to transport your prompts accross different app environments (local, acceptance, production).
* Manage the active version of a prompt that will be used by the logic in the running app.


### Prerequisites {#prerequisites}

The prerequisites of the [Conversational UI module](/appstore/modules/genai/conversational-ui/#prerequisites) apply here.

## Installation {#installation}

Follow the instructions in [How to Use Marketplace Content](/appstore/use-content/) to import the Conversational UI module into your app.

## Configuration {#configuration}

To use the Prompt Management functionality in your app, you must perform the following tasks in Studio Pro:

1. Add the relevant [module roles](#module-roles) to the applicable user roles in the project security.
2. Add the [UI to your app](#ui-components) by using the pages and snippets as a basis.
3. Make sure to have a [deployed model](#deployed-models) configured.
4. Write and test the [first prompt](#write-prompt).
5. Add the prompt to the [logic](#app-logic) of the actual use case.
6. Improve and [iterate on prompt versions](#improve-prompt).

### Configuring the Roles {#module-roles}

Make sure that the module role `PromptAdmin` is part of the user roles that are intended to do prompt management. These users design, manage and test prompts. They also decide which version is used in the running app environment. Users with the module role `User` can only read the title and description of a prompt, but not the content.

### Adding the UI to your app {#ui-components} 

A set of reusable pages, layouts, and snippets is included in this module to allow you to add the conversational UI to your app.

#### Pages and Layouts {#pages-and-layouts}

You need to include the **Prompt_Overview** page so that prompt admin users can reach it. For example by adding it to your navigation, home page or dedicated tools/settings page. 
If you require to change the layout or apply other customziations, Mendix recommends to copy the page to your own module and modify them to suit your exact app styling or use case. **Snippet_Prompt_Overview** includes the content of the same page.
From this overview, the user can reach the **Version_Details** page to edit the prompt and execute tests. If needed for customization, its contents can be found in **Snippet_Prompt_Details**.

For an example, download and run the [GenAI Showcase App](https://marketplace.mendix.com/link/component/220475) to see the pages in action.

### Configure Deployed models {#deployed-models}

You need at least one GenAI connector that follows the principles of GenAI commons to interact with LLMs from the Prompt Management logic. In order to test a prompt, at least one DeployedModel needs to be configured for your connector of choice. See the documntation of the specific connector for the exact details on how to set up the Deployed Model.

* For [Mendix Cloud GenAI](https://marketplace.mendix.com/link/component/227931), included by default, importing the **Key** from the Mendix portal automatically creates a MxCloud Deployed Model. This is part of the [configuration](/appstore/modules/genai/MxGenAI/#configuration).
* For [Amazon Bedrock](https://marketplace.mendix.com/link/component/215042), the creation of Bedrock Deployed Models is part of the [model synchronization mechanism](/appstore/modules/aws/amazon-bedrock/#sync-models).
* For [OpenAI](https://marketplace.mendix.com/link/component/220472), the configuration of OpenAI Deployed Models is part of the [configuration](/appstore/modules/genai/openai/#general-configuration)

### Write the prompt {#write-prompt}

When the app is running, a user with the role `PromptAdmin` can set up a prompt and try it out by executing a test with a deployed model. The user can decide to create either a Conversational prompt, to be used for cases where the end user would have a chat interface, or a Single-Call prompt, which is intended for isolated text generation purposes. While writing the system prompt (and for Single-Call prompt also the user prompt) the prompt engineer can include variables by enclosing them in double curly brackets, e.g., *{{variable}}*. The actual values of these placeholders are (often) only known at runtime based on the page context of the end-user. 

#### Test and refine prompt
To test the behavior of the prompts, a test can be executed. For this the prompt engineer needs to provide test values for all of the variables that were defined in the prompt(s). Additionally, it is possible to define multiple sets of test values for the variables, which can be run in bulk. Based on the outcome of the tests, the prompt engineer decides to add, remove or rephrase certain parts of the prompt.

#### Define context object
If the prompt contains variables, there needs to be an entity in your app with attributes that match with the variable names. An object of this entity functions as the context object that will contain context data and is passed when the chat completions operation is triggered (for more details see next section). It contains the actual values that need to be inserted into the prompt at the places where the variables were defined in the prompt. This entity needs to be linked to the prompt in the Prompt Management UI (for a newly created entity: make sure to run the app locally first so it is visible in the list). The PromptAdmin will see warnings on the Prompt Version details page if the attributes and variables don't match or if no entity was selected at all for the prompt. Make sure that the length of the attributes of the context object is large enough so that it can contain the actual values when logic is executed in the running app.

### Use the prompt in app logic {#app-logic}

After a number of quick iterations, a first version of the prompt is typically ready to be saved and integrated into the application logic to be tested from the end-user perspective. For this, you can add one of the operations from this module to your logic.

#### Create a version
New prompts will be created in the draft status by default. This means the prompt is still worked on and can be tested using the prompt management module only. When it is ready to be integrated in the actual app (i.e. the logic that end users trigger), the prompt must be saved as a version. This will store a snapshot of the prompt texts. It then needs to be selected as the active version for the prompt: this can be done on the prompt overview, using the menu option behind the three horizontal dots that says *Select prompt in use*.

For a Single-Call type prompt, use `Get Prompt for Context Object`, which can be found in the **Toolbox** in Studio Pro while editing a microflow, under category **GenAI (Request Building)**. This operation will yield both a system prompt and a user prompt strings, on a combined `PromptToUse` object, the string attributes of which you can pass to the chat completions operation. Retrieve the prompt (e.g. by name) and pass it with your custom context object to the operation. An example for this pattern can be found in the product description generation example of the [GenAI Showcase app](https://marketplace.mendix.com/link/component/220475).

For a conversational prompt, the chat context can be created based on the prompt in one operation. Use the `New Chat for Prompt` operation from the **Toolbox**, to be found under categroy **Conversational UI**. Retrieve the prompt (e.g. by name) and pass it with your custom context object to the operation. Note that this sets the system prompt on the chat context, and is hence applicable to the full (future) conversation. Similar to other chat context operations, an [action microflow needs to be selected](/appstore/modules/genai/conversational-ui/#action-microflow) for this microflow action.

With this microflow logic, the prompt version is ready to be tested from the end-user flow (in a local or test environment). The prompt can be exported/imported for transport to other environments if needed.

### Improve the prompt {#improve-prompt}
When a prompt version is saved, there is a button to create a new draft version. This new draft can be used as a starting point to do small changes or improvements based on feedback, either from testing or when the functionality is live for a certain amount of time and the necessity to cover additional scenarios arises.

#### Create multiple versions

The new draft version will initially have the exact same text as the latest version. The prompt texts can now be modified to cover for the additional scenarios. When the improved prompt is ready, it can be saved as a new version.

#### Manage in-use version per environment

Each time when new versions of the prompt are created, the decision needs to be made which version is to be used in the end user logic. Mendix recommendeds to evaluate the in-use version as part of the test and release process. When importing the new prompts into other environemnts, the selection of the in-use version is always a manual step and hence a conscious decision.

## Technical Reference {#technical-reference}

The module includes technical reference documentation for the available entities, enumerations, activities, and other items that you can use in your application. You can view the information about each object in context by using the **Documentation** pane in Studio Pro.

The **Documentation** pane displays the documentation for the currently selected element. To view it, perform the following steps:

1. In the [View menu](/refguide/view-menu/) of Studio Pro, select **Documentation**.
2. Click the element for which you want to view the documentation.

    {{< figure src="/attachments/appstore/platform-supported-content/modules/technical-reference/doc-pane.png" >}}

