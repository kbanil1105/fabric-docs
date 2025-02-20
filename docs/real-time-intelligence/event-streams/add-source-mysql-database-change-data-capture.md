---
title: Add MySQL Database CDC source to an eventstream
description: Learn how to add Azure Database for MySQL Change Data Capture (CDC) source to an eventstream.
ms.reviewer: spelluru
ms.author: zhenxilin
author: alexlzx
ms.topic: how-to
ms.custom:
  - ignite-2024
ms.date: 11/18/2024
ms.search.form: Source and Destination
---

# Add MySQL Database CDC source to an eventstream

>[!NOTE]
>This article contains references to the term `SLAVE`, a term that Microsoft no longer uses. When the term is removed from the software, we'll remove it from this article.

This article shows you how to add an Azure Database for MySQL Change Data Capture source to an eventstream. The Azure MySQL Database Change Data Capture (CDC) Source connector for Microsoft Fabric event streams allows you to capture a snapshot of the current data in an Azure Database for MySQL database.

You can specify the tables to monitor, and the eventstream records any future row-level changes to the tables. Once the changes are captured in the eventstream, you can process this CDC data in real-time and send it to different destinations in Fabric for further processing or analysis.

[!INCLUDE [new-sources-regions-unsupported](./includes/new-sources-regions-unsupported.md)]

## Prerequisites

- Access to a workspace in the Fabric capacity license mode (or) the Trial license mode with Contributor or higher permissions. 
- Access to an instance of Azure Database for MySQL - Flexible Server.
- Your MySQL database must be publicly accessible and not be behind a firewall or secured in a virtual network.
- If you don't have an eventstream, [create an eventstream](create-manage-an-eventstream.md). 


## Set up MySQL DB

The connector uses the Debezium MySQL connector to capture changes in your Azure Database for MySQL database. You must define a MySQL user with appropriate privileges on all databases where the Messaging Connector can capture the changes from. You can directly use the **admin user** to connect to the database which normally has the appropriate privileges already as below. or you can follow the below steps to create a new user 

> [!NOTE]
> The new user or admin account and the corresponding password will be used to connect to database later inside Eventstream. 

1. At the `mysql` command prompt, create the MySQL user:

   ```
   mysql> CREATE USER 'user'@'%' IDENTIFIED BY 'password';
   ```

1. Grant the required privileges to the user:

   ```
   mysql> GRANT SELECT, SHOW DATABASES, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'user'@'%';
   ```

1. Finalize the user's permissions:

   ```
   mysql> FLUSH PRIVILEGES;
   ```

To confirm if the user or admin has the required privileges granted, run the below command and then the required privileges in step#2 above should be shown.

```
SHOW GRANTS FOR user;
```


For more information about granting the required permissions to the user, see [Debezium connector for MySQL: Debezium Documentation](https://debezium.io/documentation/reference/stable/connectors/mysql.html#mysql-creating-user).

## Enable the binlog

You must enable binary logging for MySQL replication. The binary logs record transaction updates for replication tools to propagate changes.

1. On the Azure portal page for your Azure Database for MySQL account, select **Server parameters** under **Settings** in the left navigation.

1. On the **Server parameters** page, configure the following properties, and then select **Save**.

   - For [binlog_row_image](https://dev.mysql.com/doc/refman/8.0/en/replication-options-binary-log.html#sysvar_binlog_row_image), select **full**.

   - For [binlog_expire_logs_seconds](https://dev.mysql.com/doc/refman/8.0/en/replication-options-binary-log.html#sysvar_binlog_expire_logs_seconds), set the number of seconds the service waits before the binary log file is purged. Set the value to match the needs of your environment, for example *86400*.

   :::image type="content" border="true" source="media/add-source-mysql-database-change-data-capture/binlog.png" alt-text="A screenshot of the binlog settings for replication under Server parameters.":::

## Add Azure MySQL DB (CDC) as a source
[!INCLUDE [launch-connect-external-source](./includes/launch-connect-external-source.md)]

On the **Select a data source** page, search for and select **Connect** on the **MySQL DB (CDC)** tile.

:::image type="content" source="./media/add-source-mysql-database-change-data-capture/select-mysql-database.png" alt-text="Screenshot that shows the selection of MySQL DB (CDC) as the source type in the Get events wizard." lightbox="./media/add-source-mysql-database-change-data-capture/select-mysql-database.png":::

## Configure and connect to Azure MySQL DB (CDC) 

[!INCLUDE [mysql-database-cdc-connector](./includes/mysql-database-cdc-source-connector.md)]

[!INCLUDE [sources-destinations-note](./includes/sources-destinations-note.md)]

## View updated eventstream
1. You see the Azure MySQL DB (CDC) source added to your eventstream in **Edit mode**.

    :::image type="content" source="media/add-source-mysql-database-change-data-capture/edit-mode.png" alt-text="A screenshot of the added Azure MySQL DB CDC source in Edit mode with the Publish button highlighted." lightbox="media/add-source-mysql-database-change-data-capture/edit-mode.png":::
2. Select **Publish** to publish the changes and begin streaming Azure MySQL DB CDC data to the eventstream.

    :::image type="content" source="media/add-source-mysql-database-change-data-capture/live-view.png" alt-text="A screenshot of the added Azure MySQL DB CDC source in Live mode." lightbox="media/add-source-mysql-database-change-data-capture/live-view.png":::

## Related content

Other connectors:

- [Amazon Kinesis Data Streams](add-source-amazon-kinesis-data-streams.md)
- [Azure Cosmos DB](add-source-azure-cosmos-db-change-data-capture.md)
- [Azure Event Hubs](add-source-azure-event-hubs.md)
- [Azure IoT Hub](add-source-azure-iot-hub.md)
- [Azure SQL Database Change Data Capture (CDC)](add-source-azure-sql-database-change-data-capture.md)
- [Confluent Kafka](add-source-confluent-kafka.md)
- [Custom endpoint](add-source-custom-app.md)
- [Google Cloud Pub/Sub](add-source-google-cloud-pub-sub.md) 
- [PostgreSQL Database CDC](add-source-postgresql-database-change-data-capture.md)
- [Sample data](add-source-sample-data.md)
- [Azure Blob Storage events](add-source-azure-blob-storage.md)
- [Fabric workspace event](add-source-fabric-workspace.md)
