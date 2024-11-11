---
title: "Consumed REST Services"
url: /refguide/consumed-rest-services-beta/
description: "Describes the configuration and usage of the new Consumed REST service document."
weight: 5
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details. 
---

## Introduction

Use the Consumed REST Service document to send REST requests from Mendix Studio Pro. With this feature, you can build, test, and create data structures to store your requests. 

This feature is supported for [Mendix Studio Pro 10.17](/releasenotes/studio-pro/10.17/) and above.

### Use Cases

You can use the Consumed REST Service document to do the following:

* Consume a REST Service
* Configure `GET`, `POST`, `PUT`, `PATCH`, and `DELETE` requests
* Create entities directly in the domain model
* Send REST requests through a microflow

### Limitations

* For macOS, it is currently not possible to copy and paste in the URL or body fields. You may also experience issues while tabbing in the text field. 

### Prerequisites 

* [Studio Pro 10.17](/releasenotes/studio-pro/10.17/) and above
* Familiarity with [HTTP request methods](https://www.w3schools.com/tags/ref_httpmethods.asp)

## Add the Consumed REST Service Document {#installation}

Download [Studio Pro](https://marketplace.mendix.com/link/studiopro/) and add the Consumed REST Service document to your app. To do this, follow these steps:  

1. Right-click the module you want to add the Consumed REST Service document to.
2. Select **Add other** > **Consumed REST service**. 
3. Name the service and click **OK**.

{{< figure src="/attachments/refguide/modeling/integration/consumed-rest-services-beta/add-service.png" width="500" class="no-border" >}}

## Configuration {#configuration}

Use the Consumed REST Service to configure a `GET`, `POST`, `PUT`, `PATCH`, or `DELETE` request for your app. 

### Basic Configuration {#configure-a-request}

Create a `GET`, `POST`, `PUT`, `PATCH`, or `DELETE` request to send data to your server by doing the following:

1. In the **General** field, name your request. 
2. In the **Method & URL** field, use the drop-down to select the HTTP method you want to use.
3. Add an endpoint and click **Send**.

    {{< figure src="/attachments/refguide/modeling/integration/consumed-rest-services-beta/general-section.png" class="no-border" width="500" >}}

4. Click **Base URL**.
5. Add a base URL to use the same URL across all requests in this consumed REST Service document.

    {{< figure src="/attachments/refguide/modeling/integration/consumed-rest-services-beta/configuration-screen.png" class="no-border" width="500" >}}

    To make the base URL dynamic, see the [Dynamic Base URL](#dynamic-base-url) section below.

6. Click **Authentication**.
7. Select an authentication method, then click **OK**. For more information, see [Authentication methods](#authentication).
8. Click **Send**. 

You can visualize your request in the **Response data** tab, then use the response to [create an entity in the domain model](#create-entity). 

### Authentication Methods {#authentication}

You can configure basic authentication to use for all requests in your document. Authentication is not required, but can be added if needed. To add basic authentication, do the following:

1. Click **Authentication**.
2. In the **Authentication method** field, click the drop-down and select **Basic authentication**. 

    {{< figure src="/attachments/refguide/modeling/integration/consumed-rest-services-beta/authentication-setup.png" class="no-border" >}}

3. Select a constant or create a new one for your username and password. To create a new constant, follow these steps:
   1. Next to **Username** or **Password**, click **Select** > **New**.
   2. Name the constant and click **OK**.
4. Add any additional information needed and click **OK**.

### Adding Parameters {#add-parameters}

{{% alert color="info" %}}

Parameters are not supported in the authentication section.

{{% /alert %}}

Parameters are fully supported in the path and query part of the URL, in the header value, and in the body. They are defined within curly brackets. For example, in the URL, defining `numbers` as parameter would be `http://numbersapi.com/{numbers}`. The parameters that are configured for the URL, headers, or body within curly brackets are automatically added to the parameters grid.

{{< figure src="/attachments/refguide/modeling/integration/consumed-rest-services-beta/get-header.png" class="no-border" >}}

You can manually add new parameters to the parameters grid directly. To do so, follow these steps:

1. Open the **Parameters** tab and click **Add parameter**.
2. Name your parameter and add a test value.

    {{< figure src="/attachments/refguide/modeling/integration/consumed-rest-services-beta/parameter.png" class="no-border" >}}

3. To test the parameters, click **Send**. 

#### Dynamic Base URL {#dynamic-base-url}

You can add a Base URL as a parameter. To do this, follow these steps:

1. Click **Base URL**.
2. In the Dynamic field, select **Yes**.
3. Click **OK**

Your base URL is now considered as a parameter. You can change its value in the [Send REST Request](/refguide/send-rest-request/) mciroflow activity. 

### Adding Headers {#add-headers}

You can add a header for any HTTP request you have specified in your document. To add a header, do the following:

1. Open the **Headers** tab and click **Add header**.

    {{< figure src="/attachments/refguide/modeling/integration/consumed-rest-services-beta/header-example.png" class="no-border" width="500" >}}

2. In the **Key** field, click the drop-down and choose from the list of the most commonly used HTTP headers. You can also create a custom header by changing the key to **Custom** and adding a value in the **Value** field.

    {{< figure src="/attachments/refguide/modeling/integration/consumed-rest-services-beta/accept-header.png" class="no-border" width="500" >}}

3. Click **OK**. To test the header, click **Send**.

You can also add a parameter as the test value of a header, as seen below. For example, you can define an Authorization header where the authentication token is dynamic.

### Adding a Request Body (for POST, PUT, and PATCH requests only) {#add-a-request-body}

`POST`, `PUT`, and `PATCH` requests support JSON strings as a request body. Add the JSON body snippet to your request by doing the following:

1. Click the **Body** tab and add your JSON string.

    {{< figure src="/attachments/refguide/modeling/integration/consumed-rest-services-beta/json-example.png" class="no-border" width="500" >}}

2. To validate the input, click **Send**.

3. If you want to use the newly-created JSON string as an entity in your domain model, click **Use JSON Snippet**. The body string can be viewed in the **Body structure** tab.

    {{< figure src="/attachments/refguide/modeling/integration/consumed-rest-services-beta/json-body-structure.png" class="no-border" >}}

4. The entity name is prefilled, but you can change it to a custom name. To create an entity, click **Create Entity** > **OK**. Click **Show** to view the entity in your domain model.

### Creating an Entity from the Response {#create-entity}

You can check the response of your request in the **Response data** tab. 

{{< figure src="/attachments/refguide/modeling/integration/consumed-rest-services-beta/response-data.png" class="no-border" >}}

If you want to use the response to create an entity in your domain model, navigate to the **Response structure** tab, which displays a preview of the response data. 

{{< figure src="/attachments/refguide/modeling/integration/consumed-rest-services-beta/response-structure.png" class="no-border" >}}

The entity name is prefilled, but you can change it to a custom name. To create an entity, do the following:

1. Click **Create Entity** > **OK**. 
2. To view the entity in your domain model, click **Show**.

You can also add a parameter in the request body by generating a data structure (entities) as input. If only a small part of the request is dynamic, you can also use parameters directly in the JSON snippet.

You can choose to flatten and simplify the structure of your response. Enable this feature by selecting **Flatten and simplify structure**. This gives you an easy structure to model with within Studio Pro, removes empty entities, and merges one-to-one relations between a parent and child.

### Using a REST Request in a Microflow {#add-entity-to-microflow}

To select a request in the microflow, complete the following steps:

1. Create a new microflow and drag the [Send REST request](/refguide/send-rest-request/) activity into it.
2. Double-click the activity and click **Select** to choose the request you want to add, then click **Select** > **OK**.

{{< figure src="/attachments/refguide/modeling/integration/consumed-rest-services-beta/select-rest-request.png" class="no-border" width="500" >}}

If you have defined parameters in the request, they will be added to the activity. Click **Edit** to change the parameter in the microflow. The parameter values in this activity are used by the runtime instead of the test value defined in the request.
