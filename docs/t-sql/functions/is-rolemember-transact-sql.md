---
title: "IS_ROLEMEMBER (Transact-SQL)"
description: "IS_ROLEMEMBER (Transact-SQL)"
author: VanMSFT
ms.author: vanto
ms.date: 06/06/2022
ms.service: sql
ms.subservice: t-sql
ms.topic: reference
f1_keywords:
  - "IS_ROLEMEMBER"
  - "IS_ROLEMEMBER_TSQL"
helpviewer_keywords:
  - "roles [SQL Server], members"
  - "IS_ROLEMEMBER function"
  - "members [SQL Server], verifying"
dev_langs:
  - "TSQL"
monikerRange: ">= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || >= sql-server-linux-2017 || = azuresqldb-mi-current"
---
# IS_ROLEMEMBER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Indicates whether a specified database principal is a member of the specified database role.  
  
 ![Topic link icon](../../database-engine/configure-windows/media/topic-link.gif "Topic link icon") [Transact-SQL Syntax Conventions](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## Syntax  
  
```syntaxsql
IS_ROLEMEMBER ( 'role' [ , 'database_principal' ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## Arguments
 **'** *role* **'**  
 Is the name of the database role that is being checked. *role* is **sysname**.  
  
 **'** *database_principal* **'**  
 Is the name of the database user, database role, or application role to check. *database_principal* is **sysname**, with a default of NULL. If no value is specified, the result is based on the current execution context. If the parameter contains the word NULL will return NULL.  
 
 > [!NOTE]
 > Using an Azure AD principal for *database_principal* is not supported.
  
## Return Types  
 **int**  
  
|Return value|Description|  
|------------------|-----------------|  
|0|*database_principal* is not a member of *role*.|  
|1|*database_principal* is a member of *role*.|  
|NULL|*database_principal* or *role* is not valid, or you do not have permission to view the role membership.|  
  
## Remarks

The **IS_ROLEMEMBER** function is not supported for an Azure AD administrator when the administrator is a member of an Azure AD group. The **IS_ROLEMEMBER** function is supported for Azure AD users that are members of an Azure AD group, unless that group is the Azure AD admin.

 Use IS_ROLEMEMBER to determine whether the current user can perform an action that requires the database role's permissions.  
  
 If *database_principal* is based on a Windows login, such as Contoso\Mary5, IS_ROLEMEMBER returns NULL, unless the *database_principal* has been granted or denied direct access to [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 If the optional *database_principal* parameter is not provided and if the *database_principal* is based on a Windows domain login, it may be a member of a database role through membership in a Windows group. To resolve such indirect memberships, IS_ROLEMEMBER requests Windows group membership information from the domain controller. If the domain controller is inaccessible or does not respond, IS_ROLEMEMBER returns role membership information by accounting for the user and its local groups only. If the user specified is not the current user, the value returned by IS_ROLEMEMBER might differ from the authenticator's (such as Active Directory) last data update to [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 If the optional *database_principal* parameter is provided, the database principal that is being queried must be present in sys.database_principals, or IS_ROLEMEMBER will return NULL. This indicates that the *database_principal* is not valid in this database.  
  
 When the *database_principal* parameter is a based on a domain login or based on a Windows group and the domain controller is inaccessible, calls to IS_ROLEMEMBER will fail and might return incorrect or incomplete data.  
  
 If the domain controller is not available, the call to IS_ROLEMEMBER will return accurate information when the Windows principal can be authenticated locally, such as a local Windows account or a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] login.  
  
 **IS_ROLEMEMBER** always returns 0 when a Windows group is used as the database principal argument, and this Windows group is a member of another Windows group which is, in turn, a member of the specified database role.  
  
 The User Account Control (UAC) found in [!INCLUDE[winvista](../../includes/winvista-md.md)] and Windows Server 2008 might also return different results. This would depend on whether the user accessed the server as a Windows group member or as a specific [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] user.  
  
 This function evaluates role membership, not the underlying permission. For example, the **db_owner** fixed database role has the **CONTROL DATABASE** permission. If the user has the **CONTROL DATABASE** permission but is not a member of the role, this function will correctly report that the user is not a member of the **db_owner** role, even though the user has the same permissions.  
 
 Members of the **sysadmin** fixed server role enter every database as the **dbo** user. Checking permission for member of the **sysadmin** fixed server role, checks permissions for **dbo**, not the original login. Since **dbo** can't be added to a database role and doesn't exist in Windows groups, **dbo** will always return 0 (or NULL if the role doesn't exist).  
  
## Related Functions  
 To determine whether the current user is a member of the specified Windows group or [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database role, use [IS_MEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-member-transact-sql.md). To determine whether a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] login is a member of a server role, use [IS_SRVROLEMEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-srvrolemember-transact-sql.md).  
  
## Permissions  
 Requires VIEW DEFINITION permission on the database role.  
  
## Examples  
 The following example indicates whether the current user is a member of the `db_datareader` fixed database role.  
  
```sql  
IF IS_ROLEMEMBER ('db_datareader') = 1  
   print 'Current user is a member of the db_datareader role'  
ELSE IF IS_ROLEMEMBER ('db_datareader') = 0  
   print 'Current user is NOT a member of the db_datareader role'  
ELSE IF IS_ROLEMEMBER ('db_datareader') IS NULL  
   print 'ERROR: The database role specified is not valid.';  
```  
  
## See Also  
 [CREATE ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-role-transact-sql.md)   
 [ALTER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-role-transact-sql.md)   
 [DROP ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-role-transact-sql.md)   
 [CREATE SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-role-transact-sql.md)   
 [ALTER SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-role-transact-sql.md)   
 [DROP SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-role-transact-sql.md)   
 [IS_MEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-member-transact-sql.md)   
 [IS_SRVROLEMEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-srvrolemember-transact-sql.md)   
 [Security Functions &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
  
  
