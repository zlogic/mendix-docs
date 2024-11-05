---
title: "Integrate with Jira"
url: /developerportal/portfolio-management/integrate-with-jira/
weight: 40
description: "Describes how to integrate the Portfolio Management Tool with Jira."
---

## Introduction

Jira integration in the Portfolio Management Tool allows you to link [Jira projects](https://www.atlassian.com/software/jira/guides/projects/overview#what-is-a-jira-project) to your [portfolio](/developerportal/portfolio-management/#portfolio-landscape). With this integration, you can assign [Jira epics](https://www.atlassian.com/agile/project-management/epics) in those Jira projects to [portfolio initiatives](/developerportal/portfolio-management/initiatives-overview/#create-new-initiative) and track their progress.

### Features

* Supports connecting your portfolio to Jira.
* Allows you to link Jira projects.
* Allows you to add Jira epics.
* Allows you to view and track progress of Jira epics added to portfolio initiatives.

### Limitations

* Only Portfolio Managers from the same company as the portfolio can config Jira integration and link Jira projects to a portfolio. 
* Only Portfolio Managers and Contributors from the same company as the portfolio can add Jira epics to initiatives.
* Viewers or external members cannot use Jira integration functionality.
* This integration is only one-sided. You can only display Jira information on Portfolio Management and not vice versa. 

## Configuring Jira Integration

As a Portfolio Manager, you can integrate your portfolio with Jira. After configuring Jira, you can add Jira projects to your portfolio and link Jira epics to portfolio initiatives. Note that each portfolio requires a separate Jira integration setup.

### Prerequisites

* You need to have the Portfolio Manager role for the portfolio.
* You need to have an active subscription to [Jira Software Cloud](https://support.atlassian.com/jira-cloud-administration/docs/explore-jira-cloud-plans/).
* You need to have a project in Jira.
* You need to have a user account and API token with administration rights to the project in Jira. For more information on how to get this API token, see [Manage API tokens for your Atlassian account](https://support.atlassian.com/atlassian-account/docs/manage-api-tokens-for-your-atlassian-account/).

### Procedure

To connect your portfolio to Jira, follow these steps:

1. Open the portfolio you want to integrate with Jira. 
2. Go to the [Settings](/developerportal/portfolio-management/portfolio-settings/) page.
3. Go to the **Integrations** tab.
4. In the **Jira integration** section, click **Configure Jira Integration**. The Jira integration wizard opens to guide you through the steps to set up the integration.

     * **Jira Company Domain**: This is the URL of your company’s environment within the Jira platform as provided by Jira. This URL usually looks like this: `https://my-company.atlassian.net`.

     * **Account**: This is the login name of a user on the Jira platform with project administration rights.

     * **API Token**: This is a valid API token issued by the Jira platform and assigned to the above-mentioned admin user. For more information on how to get this API token, see [Manage API tokens for your Atlassian account](https://support.atlassian.com/atlassian-account/docs/manage-api-tokens-for-your-atlassian-account/).


5. Click **Next**.
6. Select the projects from Jira you want to make available in your portfolio. You can select up to a maximum of 20 project.
7. Click **Save**.

Once the configuration is completed, your portfolio is connected to Jira, and you can see the following:

* A card with the information details of your Jira integration and a button to [edit the current configuration](#edit-configuration). 
* A table with the Jira projects connected to your portfolio if they have been linked during the Jira integration.

## Editing Configuration {#edit-configuration}

If you want to connect to a different Jira environment or rotate the API key used by the Jira Integration, you can edit the current configuration as follows:

1. Open the portfolio you want to integrate with Jira. 
2. Go to the [Settings](/developerportal/portfolio-management/portfolio-settings/) page.
3. Go to the **Integrations** tab.
4. Click **Edit Configuration**.
5. Make the changes and save.

## Deleting Configuration

{{% alert color="info" %}}
After you delete a configuration, all linked projects an epics added from those projects are removed.
{{% /alert %}}

To delete a configuration, do the following steps:

1. Open the portfolio you want to integrate with Jira. 
2. Go to the [Settings](/developerportal/portfolio-management/portfolio-settings/) page.
3. Go to the **Integrations** tab.
4. Click the ellipsis icon (**...**) and then select **Delete**. 

   {{% todo %}}Add a screenshot.{{% /todo %}}

   A confirmation box opens to confirm your action.
   After you delete a configuration, all linked projects an epics added from those projects are removed.

## Managing Jira Projects

Once the integration with Jira has been completed, on the **Integrations** tab of the [Settings](/developerportal/portfolio-management/portfolio-settings/) page, there is a table with the list of projects linked to the portfolio. Each row contains the following project details:

* Project icon – This is the icon of the linked Jira project.
* **Name** – This is the name of the linked Jira project.
* **Key** – This is the key of the linked Jira project and clicking it will take you to the Jira project page.
* Unlink button – By clicking **⨉**, you can unlink this Jira project from your portfolio.

{{% alert color="warning" %}}
After you unlink a project, all epics added from that project will also be removed from initiatives as well.
{{% /alert %}}

### Linking a Jira Project

{{% alert color="info" %}}
You can only link Jira projects your API Token has access to, with a maximum of 20 Jira projects per portfolio.
{{% /alert %}}

Once the integration with Jira has been completed, you can link Jira projects to your portfolio as follows:

1. Open the portfolio you want to integrate with Jira. 
2. Go to the [Settings](/developerportal/portfolio-management/portfolio-settings/) page.
3. Go to the **Integrations** tab.
4. Click **+Link Projects**.
2. In the pop-up dialog box, search for and select the Jira projects you want to link to your portfolio.
3. Once your selection is complete, click **Done**. 

The Jira projects that you added appear on the **Linked Projects** list in the **Jira Configuration** section.

## Managing Jira Epics

Once you have linked Jira projects to your portfolio, you can add epics of these projects to your initiatives.
* Each initiative cannot add more than 20 Jira epics.
* External users cannot add Jira epics.

### Adding Jira Epics

To add Jira epics to an initiative, go to initiatives overview page and open the initiative side pane. Under the epics group table, you can see the Jira epics section and **+ Add Jira Epics** button.

{{% alert color="info" %}}To add Jira epics to your initiative you need to have linked Jira projects with some epics to your portfolio first.{{% /alert %}}

1. Click on **+ Add Jira Epics** button
2. In the pop-up window, first select a Jira project.
3. Then, search for your Jira epics by its full key or summary.
4. Select epics you want to add.
5. Click add button.

Now you should be able to see your added epics in the initiative side pane.

{{% alert color="info" %}}Mendix and Jira epics each have seperate limits of 20.{{% /alert %}}



