---
description: "Selecting and Configuring Affected Objects (SybaseToSQL)"
title: "Selecting and Configuring Affected Objects (SybaseToSQL) | Microsoft Docs"
ms.custom: ""
ms.date: "01/19/2017"
ms.service: sql
ms.reviewer: ""
ms.subservice: ssma
ms.topic: conceptual
helpviewer_keywords: 
  - "Tester Component,Affected Objects"
ms.assetid: a219df74-543a-4aec-aeeb-79f90ac3e2ee
author: cpichuka 
ms.author: cpichuka 
---
# Selecting and Configuring Affected Objects (SybaseToSQL)
At this page you can select tables and foreign keys, changes in which should be compared when SSMA verifies the results of execution for the objects chosen in the previous step. Also, you can customize the verification parameters.  
  
## Selection of Affected Objects  
In the Sybase object tree located on the left side of the window, check the tables and foreign keys, changes in which should be compared for being identical.  
  
If SSMA Tester cannot verify any of these objects, you will see the link labeled **Some selected objects contain errors** under the objects tree. Click this link to view the reasons why these objects cannot be compared and to clear the selection of wrong objects.  
  
## Table  
The Table tab contains the grid view of the table selected. The grid contains the following information about the selected table:  
  
-   Column Name  
  
-   Data Type  
  
-   Precision  
  
-   Scale  
  
-   Rule  
  
-   Default  
  
-   Identity  
  
-   Nullable  
  
## Sql  
SQL tab contains the "Create table" SQL of the table selected.  
  
## Data  
Data tab displays data present in the table selected.  
  
## Properties  
Properties tab displays Properties of the selected table. The following fields are present under the Properties tab:  
  
-   Created or Last Modified  
  
-   Object Name  
  
## Table Comparison Settings  
Establish the comparison rules for table on **Table Comparisons** page. You can make the following settings.  
  
### Comparison mode  
Defines the table content on which will be performed the comparison.  
  
-   If you select **Changes Only**, full comparison of table rows will be performed.  
  
-   If you select **Full**, the comparison for only the rows that were changed will be performed.  
  
## Column Comparison Settings  
Establish the comparison rules for table columns on **Column Comparisons** page. You can make the following settings.  
  
### Use During Test Comparisons  
Determine if this column will participate in test results verification.  
  
-   If you choose **True**, SSMA will compare the contents of this column after executing the test on Sybase with the contents of the column in SQL Server.
  
-   If you choose **False**, the column will be excluded from results verification.  
  
### Use Custom Scale  
For columns of numeric data type, you can set a custom scale for the comparison.  
  
-   If you choose **True**, numeric values will be rounded according to the **Comparing Scale** value before they are compared.  
  
-   If you choose **False**, the numeric comparison will be exact.  
  
### Comparing Scale  
  
-   Available only if the **Use Custom Scale** option is set to **True**. This is the precision for numeric comparison.  
  
### Date Time Comparing  
Defines how date/time values are compared.  
  
-   If you select **Compare Whole Date**, full comparison of values from both platforms will be performed.  
  
-   If you select **Compare Only Date**, the time part will be ignored.  
  
-   If you select **Compare Only Time**, the date part will be ignored.  
  
-   If you select **Ignore Milliseconds**, the results will be compared up to seconds.  
  
-   If you select **Ignore Date and Milliseconds**, the result will be compared only by time part and ignoring fractional parts of a second.  
  
### Ignore Strings Case  
Controls the comparison's case sensitivity.  
  
-   If you choose **True**, the comparison will be case insensitive.  
  
-   If you choose **False**, the comparison will account for letter case.  
  
## Comparing SQL  
You can view the SELECT statements generated by SSMA Tester on the **Compare SQL** page. The Tester will compare the result sets of these statements on a row-by-row basis. Each next row of a Sybase result set should be equal to the next row of the result set produced in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
You can edit those SELECT statements to provide custom verification. To save the changes in Sybase and in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] statements, use the **Apply** buttons under the source and target SQL, correspondingly.  
  
## Next Step  
[Customizing Calls Order &#40;SybaseToSQL&#41;](../../ssma/sybase/customizing-calls-order-sybasetosql.md)  
  
## See Also  
[Running Test Cases &#40;SybaseToSQL&#41;](../../ssma/sybase/running-test-cases-sybasetosql.md)  
[Testing Migrated Database Objects &#40;SybaseToSQL&#41;](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md)  
  
