---
title: Subscribe to Grafana Enterprise
description:  Activate Grafana Enterprise to access Grafana Enterprise plugins within Azure Managed Grafana
author: maud-lv
ms.author: malev
ms.service: azure-managed-grafana
ms.topic: how-to 
ms.date: 04/16/2025
---

# Enable Grafana Enterprise

In this guide, learn how to activate the Grafana Enterprise add-on in Azure Managed Grafana, update your Grafana Enterprise plan, and access [Grafana Enterprise plugins](https://grafana.com/docs/plugins/).

The Grafana Enterprise plans offered through Azure Managed Grafana enable users to access Grafana Enterprise plugins to do more with Azure Managed Grafana.

>[!NOTE]
> To activate the Grafana Enterprise option, your Azure Managed Grafana workspace must be using the Standard plan. For more information about plans, go to [pricing plans](overview.md#service-tiers).

Grafana Enterprise plugins, as of March 2025:

- Adobe Analytics
- Amazon Aurora
- AppDynamics
- Atlassian Statuspage
- Azure Cosmos DB
- Azure DevOps
- Catchpoint
- Cloudflare
- CockroachDB
- Databricks
- Datadog
- Drone
- DynamoDB
- Dynatrace
- GitLab
- Grafana Enterprise Logs
- Grafana Enterprise Metrics
- Grafana Enterprise Traces
- Honeycomb
- Jira
- Looker
- MongoDB
- Netlify
- New Relic
- Oracle Database
- PagerDuty
- Salesforce
- SAP HANA®
- ServiceNow
- Snowflake
- Splunk
- Splunk Infrastructure Monitoring
- Sqlyze Datasource
- Sumo Logic
- Vercel
- Wavefront
- Zendesk

> [!NOTE]
> Grafana Enterprise plugins are directly supported by Grafana Labs. For more information and an updated list, go to [Grafana Enterprise plugins](https://grafana.com/docs/plugins/).

You can enable access to Grafana Enterprise plugins by selecting a Grafana Enterprise plan when creating a new workspace, or you can add a Grafana Enterprise plan on an already-created Azure Managed Grafana workspace.

> [!NOTE]
> The Grafana Enterprise monthly plan is a paid plan, owned and charged by Grafana Labs, through Azure Marketplace. Go to [Azure Managed Grafana pricing](https://azure.microsoft.com/pricing/details/managed-grafana/) for details.

> [!NOTE]
> Azure Managed Grafana Enterprise is only available in Azure Global.

## Prerequisites

- An Azure account with an active subscription. [Create an account for free](https://azure.microsoft.com/free).
    > [!NOTE]
    > Only Azure subscriptions billed directly through Microsoft are eligible for the purchase of Grafana Enterprise. CSP subscriptions, i.e., Azure subscriptions billed through Cloud Solution Providers (CSP), are ineligible.
- This guide assumes that you already know the basics of [creating an Azure Managed Grafana workspace](quickstart-managed-grafana-portal.md).

## Create a workspace with Grafana Enterprise enabled

When [creating a new Azure Managed Grafana workspace](quickstart-managed-grafana-portal.md) and filling out the **Basics** tab of the creation form, follow the steps below:

1. Under **Pricing Plans**, select the **Standard** plan and the instance size of your choice.
1. Under **Grafana Enterprise**, check the box **Grafana Enterprise**, select **Free Trial - Grafana Enterprise Plugins on Azure Managed Grafana**, and keep the option **Recurring billing** on **Disabled**.

    > [!CAUTION]
    > Each Azure subscription can benefit from one and only one free Grafana Enterprise trial. The free trial lets you try the Grafana Enterprise plan for one month.
    > - Grafana Enterprise plugins will be disabled once the free trial expires. Enterprise-plugin based data sources and dashboards created during the free trial period will no longer work after the expiration of the free trial. To use those data sources and dashboards, you will need to purchase a paid plan.
    > - If you delete a Grafana Enterprise free trial resource, you will not be able to create another Grafana Enterprise free trial. Free trial is for one-time use only.

1. Select **Review + create** and review the information about your new workspace, including the costs that may be associated with the Grafana Enterprise plan and potential other paid options.
  
1. Read and check the box at the bottom of the page to state that you agree with the terms displayed, and select **Create** to finalize the creation of your new Azure Managed Grafana workspace.

## Activate Grafana Enterprise on an existing workspace

To enable Grafana Enterprise on an existing Azure Managed Grafana workspace, follow the steps below:

  1. In the Azure portal, open your Grafana workspace and under **Settings**, select **Grafana Enterprise**.

  1. Select **Free Trial - Grafana Enterprise Plugins on Azure Managed Grafana** to test Grafana Enterprise for free or select another plan. Review the associated costs to make sure that you selected a plan that suits you. Optionally enable recurring billing.
  
        :::image type="content" source="media/grafana-enterprise/enable-grafana-enterprise.png" alt-text="Screenshot of the Grafana dashboard showing how to enable Grafana enterprise on an existing workspace." lightbox="media/grafana-enterprise/enable-grafana-enterprise.png":::
  
        > [!CAUTION]
        > Each Azure subscription can benefit from one and only one free Grafana Enterprise trial. The free trial lets you try the Grafana Enterprise plan for one month.
        > - Grafana Enterprise plugins are disabled once the free trial expires. Enterprise-plugin based data sources and dashboards created during the free trial period no longer work after the expiration of the free trial. To use those data sources and dashboards, you will need to purchase a paid plan.
        > - If you delete a Grafana Enterprise free trial resource, you will not be able to create another Grafana Enterprise free trial. Free trial is for one-time use only.

  1. Read and check the box at the bottom of the page to state that you agree with the terms displayed, and select **Update** to finalize the update.

## Update a Grafana Enterprise plan

To update the Grafana Enterprise plan of an existing Azure Managed Grafana workspace, follow the steps below:

  1. In the Azure portal, open your Grafana workspace and under **Settings**, select **Grafana Enterprise**. Review the available information about your current plan, price and billing.
  1. Select **Change plan** to review available Grafana Enterprise plans and select another plan. Select **Update** at the bottom of the page to switch to the selected plan.
  1. Select **Edit recurring billing** to enable or disable the recurring billing and renewal of your Grafana Enterprise plan. The subscription expires on the date displayed on screen. To confirm, select **Update**.
  1. Select **Cancel enterprise** option to cancel the Grafana Enterprise subscription. Enter the name of the plan and select **Cancel enterprise** again to confirm.

      > [!NOTE]
      > If you configure Grafana Enterprise data sources and later cancel your subscription, you will no longer have access to them. Your Grafana Enterprise data sources and associated dashboards will remain in your Grafana workspace but you will need to subscribe again to Grafana Enterprise to regain access to your data.

The Azure platform displays some useful links at the bottom of the page.

## Start using Grafana Enterprise plugins

Grafana Enterprise gives you access to preinstalled plugins reserved for Grafana Enterprise customers. Once you've activated a Grafana Enterprise plan, go to the Grafana platform and select **Connections > Connect data** from the left menu to create a new connection using the newly accessible data sources.

:::image type="content" source="media/grafana-enterprise/access-data-sources.png" alt-text="Screenshot of the Grafana dashboard. Access data sources.":::

## Next step

In this how-to guide, you learned how to enable Grafana Enterprise plugins. To learn how to configure data sources, go to:

> [!div class="nextstepaction"]
> [Configure data sources](how-to-data-source-plugins-managed-identity.md)
