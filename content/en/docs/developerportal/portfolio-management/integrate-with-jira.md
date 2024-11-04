---
title: "Integrate with Jira"
url: /developerportal/portfolio-management/integrate-with-jira/
weight: 40
description: "Describes how to integrate the Portfolio Management Tool with Jira."
---

## Introduction

Jira integration allows you to link [Jira projects](https://www.atlassian.com/software/jira/guides/projects/overview#what-is-a-jira-project) to your [portfolio](/developerportal/portfolio-management/#portfolio-landscape). With this integration you can assign [Jira epics](https://www.atlassian.com/agile/project-management/epics) of those Jira projects to portfolio [initiatives](/developerportal/portfolio-management/initiatives-overview/#create-new-initiative) and track their progress. 

### Features

* Supports connecting your portfolio to Jira.
* Allows you to link Jira projects.
* Allows you to add Jira epics.
* Allows you to view and track progress of Jira epics added to portfolio initiatives.

### Limitations

* Only Portfolio Managers from the same company as the portfolio can config Jira integration and link Jira projects to a portfolio. 
* Only Portfolio Managers and Contributors from the same company as the portfolio can add Jira epics to initiatives.
* Viewers or external members cannot use Jira integration functionality.
* This integration is only one-sided. You can only display Jira information on Portfolio Management and not vice-versa. 

## Configure Jira Integration

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
3. Go to the Integrations tab.
4. In the Jira integration section, click on the button Configure Jira Integration. The Jira integration wizard will open with the instructions and fill in the information as follows:

  * Jira Company Domain: This is the URL of your company’s environment within the Jira platform as provided by Jira. This URL usually looks like this: https://my-company.atlassian.net.
  * Account: This is the login name of a user on the Jira platform with project administration rights.
  * API Token: This is a valid API token issued by the Jira platform and assigned to the above-mentioned admin user. For more information on how to get this API token, see Manage API tokens for     your Atlassian account.
  
5. Click Next.
6. Select the projects from Jira you want to make available in your portfolio.
7. Click Save.

Once the configuration is completed, your portfolio is connected to Jira, and you should be able to see the following:

* A card with the information details of your Jira integration and a button to edit the current configuration. 
* A table with the Jira projects connected to your portfolio if they have been linked during the Jira integration.

#### Edit Configuration

If you need to connect to a different Jira environment or need to rotate the API key used by the Jira Integration, click on Edit Configuration.

#### Delete Configuration

You can delete a configuration by first clicking on the 3 dot menu icon on the right and then delete button. You will get a confirmation box to confirm your action.
Deleting a configuration will cause all linked projects and added epics of those projects to be removed as well.

## Managing Projects

Once the integration with Jira has been completed, there should be a table with the list of projects linked to the portfolio. Each row contains project details, such as:

* **Icon** - This is the icon of the linked Jira project.
* **Name** - This is the name of the linked Jira project.
* **Key** - This is the key of the linked Jira project and clicking it will take you to the Jira project page.
* **Unlink button** -  By clicking this x button, you can unlink this Jira project from your portfolio.

{{% alert color="info" %}}When you delete a project, all added epics of that project are going to be removed from initiatives as well.{{% /alert %}}

### Linking a Jira Project

Once the integration with Jira has been completed, you can link Jira projects to your portfolio. On the integration tab, you can find the button to link projects beneath the Linked Projects table.

{{% alert color="info" %}You can only link projects your API Token has access to, with a maximum of 20 projects per portfolio.{{% /alert %}}

1. Click on +Link Projects button
2. In the pop-up window, search for and select the Jira projects you want to link to your portfolio.
3. Once your selection is complete, click on Done and the recently added projects should appear on the Linked Projects table in the Jira Configuration section.

## Manage Jira Epics

Once you have linked Jira projects to your portfolio, you can add epics of these projects to your initiatives.
* Each initiative can not add more than 20 Jira epics.
* External users cannot add Jira epics.

In the initiative side pane, you can view your already added Jira epics. Each epic row contains the following information:

* **Icon** - This is the icon of the Jira project that the epic belongs to.
* **Key** - This is the key of the added Jira epic, and clicking it will take you to the Jira page of this epic.
* **Summary** - This is the summary of the added Jira epic.
* **Assignee** - This shows the avatar of the the epic assignee. Hovering over will display their user name.
* **Progress** - This shows the progress of the epic by displaying total and completed Jira stories.
* **Remove button** -  By clicking this x button, you can remove this Jira epic from your initiative.

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



