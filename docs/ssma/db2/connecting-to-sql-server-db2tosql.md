---
description: "Connecting to SQL Server (DB2ToSQL)"
title: "Connecting to SQL Server (DB2ToSQL) | Microsoft Docs"
ms.service: sql
ms.custom: ""
ms.date: "11/16/2020"
ms.reviewer: ""
ms.subservice: ssma
ms.topic: conceptual
ms.assetid: b59803cb-3cc6-41cc-8553-faf90851410e
author: cpichuka 
ms.author: cpichuka 
---

# Connecting to SQL Server (DB2ToSQL)

To migrate DB2 databases to [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], you must connect to the target instance of the [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. When you connect, SSMA obtains metadata about all the databases in the instance of [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] and displays database metadata in the **SQL Server Metadata Explorer**. SSMA stores information about which instance of [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] you are connected to, but does not store passwords.

Your connection to [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stays active until you close the project. When you reopen the project, you must reconnect to [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] if you want an active connection to the server. You can work offline until you load database objects into [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] and migrate data.

Metadata about the instance of [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] is not automatically synchronized. Instead, to update the metadata in **SQL Server Metadata Explorer**, you must manually update the [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] metadata. For more information, see the "Synchronizing SQL Server Metadata" section later in this topic.

## Required SQL Server Permissions

The account that is used to connect to [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requires different permissions depending on the actions that the account performs:

- To convert DB2 objects to [!INCLUDE[tsql](../../includes/tsql-md.md)] syntax, to update metadata from [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], or to save converted syntax to scripts, the account must have permission to log on to the instance of [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

- To load database objects into [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], the account must be a member of the **db_ddladmin** server role.

- To migrate data to [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], the account must be a member of the **db_owner** database role.

- To run the code that is generated by SSMA, the account must have `EXECUTE` permissions for all user-defined functions in the **ssma_db2** schema of the target database. These functions provide equivalent functionality of DB2 system functions, and are used by converted objects.

## Establishing a SQL Server Connection

Before you convert DB2 database objects to [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] syntax, you must establish a connection to the instance of [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] where you want to migrate the DB2 database or databases.

When you define the connection properties, you also specify the database where objects and data will be migrated. You can customize this mapping at the DB2 schema level after you connect to [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. For more information, see [Mapping DB2 Schemas to SQL Server Schemas &#40;DB2ToSQL&#41;](../../ssma/db2/mapping-db2-schemas-to-sql-server-schemas-db2tosql.md).

> [!IMPORTANT]
> Before you try to connect to [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], make sure that the instance of [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] is running and can accept connections.  
  
To connect to the [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:

1. On the **File** menu, select **Connect to SQL Server**.
   If you previously connected to [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], the command name will be **Reconnect to SQL Server**.

2. In the connection dialog box, enter or select the name of the instance of [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
   - If you are connecting to the default instance on the local computer, you can enter `localhost` or a dot (`.`).
   - If you are connecting to the default instance on another computer, enter the name of the computer.
   - If you are connecting to a named instance on another computer, enter the computer name followed by a backslash and then the instance name, such as `MyServer\MyInstance`.

3. If your instance of [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] is configured to accept connections on a non-default port, enter the port number that is used for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connections in the **Server port** box. For the default instance of [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], the default port number is 1433. For named instances, SSMA will try to obtain the port number from the [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser Service.

4. In the **Database** box, enter the name of the target database.
   This option is not available when you reconnect to [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

5. In the **Authentication** box, select the authentication type to use for the connection. To use the current Windows account, select **Windows Authentication**. To use a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] login, select **SQL Server Authentication**, and then provide the login name and password.

6. For Secure connection, two controls are added, the **Encrypt Connection** and **TrustServerCertificate** check boxes. Only when **Encrypt Connection** is checked, the **TrustServerCertificate** check box is visible. When **Encrypt Connection** is checked (true) and **TrustServerCertificate** is unchecked (false), it will validate the SQL Server SSL certificate. Validating the server certificate is a part of the SSL handshake and ensures that the server is the correct server to connect to. To ensure this, a certificate must be installed on the client side as well as on the server side.

7. Click **Connect**.

> [!IMPORTANT]
> While you may connect to a higher version of [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], compared to the version chosen when the migration project was created, conversion of the database objects is determined by the target version of the project and not the version of the [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] you are connected to.

## Synchronizing SQL Server Metadata

Metadata about [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] databases is not automatically updated. The metadata in **SQL Server Metadata Explorer** is a snapshot of the metadata when you first connected to [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], or the last time that you manually updated metadata. You can manually update metadata for all databases, or for any single database or database object. To synchronize metadata:

1. Make sure that you are connected to [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
2. In **SQL Server Metadata Explorer**, select the check box next to the database or database schema that you want to update.
   For example, to update the metadata for all databases, select the box next to **Databases**.

3. Right-click **Databases**, or the individual database or database schema, and then select **Synchronize with Database**.

## Next Step

The next step in the migration depends on your project needs:

- To customize the mapping between DB2 schemas and [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] databases and schemas, see [Mapping DB2 Schemas to SQL Server Schemas &#40;DB2ToSQL&#41;](../../ssma/db2/mapping-db2-schemas-to-sql-server-schemas-db2tosql.md).
- To customize configuration options for the projects, see [Project Settings &#40;Conversion&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-conversion-db2tosql.md) and related sections.
- To customize the mapping of source and target data types, see [Mapping DB2 and SQL Server Data Types &#40;DB2ToSQL&#41;](../../ssma/db2/mapping-db2-and-sql-server-data-types-db2tosql.md).
- If you do not have to perform any of these tasks, you can convert the DB2 database object definitions into [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] object definitions. For more information, see [Converting DB2 Schemas &#40;DB2ToSQL&#41;](../../ssma/db2/converting-db2-schemas-db2tosql.md).

## See Also

[Migrating DB2 Databases to SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)
