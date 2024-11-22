---
title: "Troubleshoot Fabric Mirrored Databases From Azure SQL Managed Instance (Preview)"
description: Troubleshooting for mirrored databases from Azure SQL Managed Instance in Microsoft Fabric.
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: lazartimotic, jingwang, nzagorac
ms.date: 11/19/2024
ms.topic: troubleshooting
---
# Troubleshoot Fabric mirrored databases from Azure SQL Managed Instance (Preview)

This article covers troubleshooting steps troubleshooting for mirroring Azure SQL Managed Instance.

## Changes to Fabric capacity or workspace

| Cause    | Result | Recommended resolution     |
|:--|:--|:--|
| Fabric capacity paused/deleted | Mirroring stops | 1. Resume or assign capacity from the Azure portal <br> 2. Go to Fabric mirrored database item. From the toolbar, select **Stop replication**.<br> 3. Start replication by selecting **Mirror database** for the mirrored item in the Fabric portal. |
| Fabric capacity resumed | Mirroring isn't resumed | 1. Go to Fabric mirrored database item. From the toolbar, select **Stop replication**. <br> 2. Start replication by selecting **Mirror database** for the mirrored item in the Fabric portal. |
| Workspace deleted | Mirroring stops automatically | 1. If mirroring is still active on the Azure SQL Managed Instance, execute the following stored procedure on your Azure SQL Managed Instance: `exec sp_change_feed_disable_db;`. |
| Fabric trial capacity expired |  Mirroring stops automatically | See [Fabric trial capacity expires](../../get-started/fabric-trial.md#the-trial-expires). |

## T-SQL queries for troubleshooting

If you're experiencing mirroring problems, perform the following database level checks using Dynamic Management Views (DMVs) and stored procedures to validate configuration.

1. Execute the following query to check if the changes properly flow:

    ```sql
    SELECT * FROM sys.dm_change_feed_log_scan_sessions;
    ```

1. If the `sys.dm_change_feed_log_scan_sessions` DMV doesn't show any progress on processing incremental changes, execute the following T-SQL query to check if there are any problems reported:

    ```sql
    SELECT * FROM sys.dm_change_feed_errors;
    ```

1. If there aren't any issues reported, execute the following stored procedure to review the current configuration of the mirrored Azure SQL Managed Instance. Confirm it was properly enabled.

    ```sql
    EXEC sp_help_change_feed;
    ```

    The key columns to look for here are the `table_name` and `state`. Any value besides `4` indicates a potential problem. (Tables shouldn't sit for too long in statuses other than `4`)

1. If replication is still not working, verify that the correct SAMI object has permissions (see [SPN permissions](#spn-permissions)).
    1. In the Fabric portal, select the "..." ellipses option on the mirrored database item.
    1. Select the **Manage Permissions** option.
    1. Confirm that the Azure SQL Managed Instance name shows with Read, Write permissions.
    1. Ensure that AppId that shows up matches the ID of the SAMI of your Azure SQL Managed Instance.

1. [Contact support](/power-bi/support/service-support-options) if troubleshooting is required.

## Managed identity

The System Assigned Managed Identity (SAMI) of the Azure SQL Managed Instance needs to be enabled, and must be the primary identity.

After enablement, if SAMI setting status is either turned Off or initially enabled, then disabled, and then enabled again, the mirroring of Azure SQL Managed Instance to Fabric OneLake will fail. SAMI after re-enabling isn't the same identity as before disabling. Therefore, you need to grant the new SAMI permissions to access the Fabric workspace.

The SAMI must be the primary identity. Verify the SAMI is the primary identity with the following SQL: `SELECT * FROM sys.dm_server_managed_identities;`

User Assigned Managed Identity (UAMI) isn't supported. If you add a UAMI, it becomes the primary identity, replacing the SAMI as primary. This causes replication to fail. To resolve:

- Remove all UAMIs. Verify that the SAMI is enabled.

## SPN permissions

Don't remove Azure SQL Managed Instance service principal name (SPN) contributor permissions on Fabric mirrored database item.

If you accidentally remove the SPN permission, mirroring Azure SQL Managed Instance won't function as expected. No new data can be mirrored from the source database.

If you remove Azure SQL Managed Instance SPN permissions or permissions aren't set up correctly, use the following steps.

1. Add the SPN as a user by selecting the `...` ellipses option on the mirrored managed instance item.
1. Select the **Manage Permissions** option.
1. Enter the Azure SQL Managed Instance public endpoint. Provide **Read** and **Write** permissions.

## Related content

- [Limitations in Microsoft Fabric mirrored databases from Azure SQL Managed Instance (Preview)](azure-sql-managed-instance-limitations.md)
- [Frequently asked questions for Mirroring Azure SQL Managed Instance in Microsoft Fabric (Preview)](azure-sql-managed-instance-faq.yml)