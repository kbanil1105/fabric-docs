---
title: Get data for Activator from Power BI
description: Learn how to get data from Power BI for use in Activator, integrate it into your workflows, and take advantage of powerful data analysis capabilities.
author: mihart
ms.author: mihart
ms.topic: how-to
ms.custom: FY25Q1-Linter, ignite-2024
ms.date: 11/08/2024
ms.search.form: Activator PBI Onramp
#customer intent: As a Power BI user I want to learn how to get data for Activator in Power BI.
---

 # Get data for [!INCLUDE [fabric-activator](../includes/fabric-activator.md)] from Power BI

You can get data for use in Fabric [!INCLUDE [fabric-activator](../includes/fabric-activator.md)] from many sources. This article describes how to set alerts from data in Power BI reports.

You can use [!INCLUDE [fabric-activator](../includes/fabric-activator.md)] to set alerts when conditions are met in data in a Power BI report. For example, if you have a report displaying daily sales per store, you can set an alert to notify you if daily sales for any store fall beneath a threshold you set. You can send notifications to yourself or to others in your organization. This section explains how to define and create alerts.

## Prerequisites

Before you begin:

* You need a Power BI report that is published online to a workspace.
* The report must contain live data.

## Create an [!INCLUDE [fabric-activator](../includes/fabric-activator.md)] rule for a Power BI report visual

Open a report and select a visual to monitor. Create an [!INCLUDE [fabric-activator](../includes/fabric-activator.md)] rule that sets conditions for sending a notification. The notification can be sent in email or in Microsoft Teams.

1. Open your Power BI report.
2. Choose a visual on the report for [!INCLUDE [fabric-activator](../includes/fabric-activator.md)] to monitor.
3. Select the ellipsis (…) at the top-right of the visual, and choose **Set alert**. You can also select the bell icon in the visual, or use the **Set alert** button in the Power BI menu bar.

The following image shows an example of how to set an alert from a visual that displays daily sales by store:

:::image type="content" source="media/activator-get-data/data-activator-get-data-01.png" alt-text="Screenshot of sales by store in Power BI report.":::

4. Enter the condition to monitor. For example, select **Sales** as the measure and set a rule to notify you via email when the value drops less than $1,000. 

    * If your visual has multiple series, then [!INCLUDE [fabric-activator](../includes/fabric-activator.md)] applies the alert rule to each series. In the example shown here, the visual shows sales per store, so the alert rule applies per store.
    * If your visual has a time axis, then [!INCLUDE [fabric-activator](../includes/fabric-activator.md)] uses the time axis in the alert logic. In the example shown here, the visual has a daily time axis, so [!INCLUDE [fabric-activator](../includes/fabric-activator.md)] monitors sales per day. [!INCLUDE [fabric-activator](../includes/fabric-activator.md)] checks each point on the time axis once. If the visual updates the value for a particular point in time after [!INCLUDE [fabric-activator](../includes/fabric-activator.md)] checks it, then [!INCLUDE [fabric-activator](../includes/fabric-activator.md)] ignores the updated value.
    * You can create alerts on tables and matrix visuals. [!INCLUDE [fabric-activator](../includes/fabric-activator.md)] applies the alert condition to each row in the table, or to each cell in the matrix. If your table or matrix has a column containing timestamps, then [!INCLUDE [fabric-activator](../includes/fabric-activator.md)] interprets that column as a time axis.
    * [!INCLUDE [fabric-activator](../includes/fabric-activator.md)] uses the filters in place at the time that you create your alert. Changing the filters on your visual after creating your alert has no effect on the alert logic. Select **Show applied filters** to see the filters on your visual.

5. When you're ready to save your alert, select **Create.** Selecting **Create** saves the alert condition in an [!INCLUDE [fabric-activator](../includes/fabric-activator.md)] item. Optionally, you can select **Show save options** to specify the location of the [!INCLUDE [fabric-activator](../includes/fabric-activator.md)] item. After you create your alert, [!INCLUDE [fabric-activator](../includes/fabric-activator.md)] monitors the data in the visual once per hour. You receive an alert within one hour of your alert condition becoming true.

    :::image type="content" source="media/activator-get-data/data-activator-get-data-02.png" alt-text="Screenshot of create an alert window showing daily sales rule.":::

### Optional: edit your rule in [!INCLUDE [fabric-activator](../includes/fabric-activator.md)]

When your rule is ready, Power BI notifies you and gives you the option to open your rule in [!INCLUDE [fabric-activator](../includes/fabric-activator.md)] for further editing.

:::image type="content" source="media/activator-get-data/data-activator-get-data-03.png" alt-text="Screenshot of rule successfully created.":::

Use [!INCLUDE [fabric-activator](../includes/fabric-activator.md)] to fine tune your rule and set complex conditions that are more granular than is possible in Power BI. Another reason to use [!INCLUDE [fabric-activator](../includes/fabric-activator.md)] is if you want to trigger a Power Automate flow when your rule is activated. Refer to [Create rules in design mode](activator-create-activators.md) for information on how to edit rules in [!INCLUDE [fabric-activator](../includes/fabric-activator.md)].

## Related content

* [What is [!INCLUDE [fabric-activator](../includes/fabric-activator.md)]?](activator-introduction.md)
* [Use Custom Actions to trigger Power Automate Flows](activator-trigger-power-automate-flows.md)
* [[!INCLUDE [fabric-activator](../includes/fabric-activator.md)] tutorial using sample data](activator-tutorial.md)

You can also learn more about Microsoft Fabric:

* [What is Microsoft Fabric?](../../get-started/microsoft-fabric-overview.md)
