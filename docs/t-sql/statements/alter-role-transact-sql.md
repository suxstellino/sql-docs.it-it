---
description: ALTER ROLE (Transact-SQL)
title: ALTER ROLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_ROLE_TSQL
- ALTER ROLE
dev_langs:
- TSQL
helpviewer_keywords:
- modifying database roles
- ALTER ROLE statement
- renaming database roles
- database roles [SQL Server], modifying
- names [SQL Server], database roles
ms.assetid: e1e83caa-17cc-4871-b2db-2711339fb64f
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ed5da4d728d142a4b80aa894a57ed342368ce09e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204084"
---
# <a name="alter-role-transact-sql"></a>ALTER ROLE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Aggiunge o rimuove i membri a o da un ruolo del database, oppure modifica il nome di un ruolo del database definito dall'utente.  
  
> [!NOTE]  
>  Per modificare i ruoli aggiungendo o eliminando i membri in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], usare [sp_addrolemember &#40;Transact-SQL&#41; ](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md) e [sp_droprolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server (starting with 2012) and Azure SQL Database  
  
ALTER ROLE  role_name  
{  
       ADD MEMBER database_principal  
    |  DROP MEMBER database_principal  
    |  WITH NAME = new_name  
}  
[;]  
```  
  
 
```syntaxsql
-- Syntax for SQL Server 2008, Azure Synapse Analytics and Parallel Data Warehouse
  
-- Change the name of a user-defined database role  
ALTER ROLE role_name   
    WITH NAME = new_name  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *role_name*  
 **SI APPLICA A:**  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire dalla versione 2008), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Specifica il ruolo del database da modificare.  
  
 ADD MEMBER *database_principal*  
 **SI APPLICA A:**  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire dalla versione 2012), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Specifica di aggiungere l'entità di sicurezza del database all'appartenenza di un ruolo del database.  
  
-   *database_principal* è un utente o un ruolo del database definito dall'utente.  
  
-   *database_principal* non può essere un ruolo predefinito del database o un'entità di sicurezza del server.  
  
DROP MEMBER *database_principal*  
 **SI APPLICA A:**  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire dalla versione 2012), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Specifica di rimuovere un'entità di sicurezza del database dall'appartenenza di un ruolo del database.  
  
-   *database_principal* è un utente o un ruolo del database definito dall'utente.  
  
-   *database_principal* non può essere un ruolo predefinito del database o un'entità di sicurezza del server.  
  
WITH NAME = *new_name*  
 **SI APPLICA A:**  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire dalla versione 2008), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Specifica di modificare il nome di un ruolo del server definito dall'utente. Il nuovo nome non deve essere già esistente nel database.  
  
 La modifica del nome di un ruolo del database non comporta la modifica del numero di ID, del proprietario o delle autorizzazioni del ruolo.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire questo comando sono necessarie una più delle autorizzazioni o appartenenze seguenti:  
  
-   Autorizzazione **ALTER** per il ruolo  
-   È necessaria l'autorizzazione **ALTER ANY USER** per il database  
-   Appartenenza al ruolo predefinito del database **db_securityadmin**  
  
Per modificare l'appartenenza a un ruolo predefinito del database è anche necessaria l'appartenenza seguente:  
  
-   Appartenenza al ruolo predefinito del database **db_owner**  
  
## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
 Non è possibile modificare il nome di un ruolo predefinito del database.  
  
## <a name="metadata"></a>Metadati  
 Queste viste di sistema contengono informazioni sui ruoli del database e sulle entità di sicurezza del database.  
  
-   [sys.database_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)  
  
-   [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-change-the-name-of-a-database-role"></a>R. Modificare il nome di un ruolo del database  
 **SI APPLICA A:**  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire dalla versione 2008), [!INCLUDE[ssSDS](../../includes/sssds-md.md)]  
  
 Nell'esempio seguente viene modificato il nome del ruolo `buyers` in `purchasing`.   Questo esempio può essere eseguito nel database di esempio [AdventureWorks](https://msftdbprodsamples.codeplex.com/).
  
```sql  
ALTER ROLE buyers WITH NAME = purchasing;  
```  
  
### <a name="b-add-or-remove-role-members"></a>B. Aggiungere o rimuovere i membri del ruolo  
 **SI APPLICA A:**  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire dalla versione 2012), [!INCLUDE[ssSDS](../../includes/sssds-md.md)]  
  
 In questo esempio viene creato un nuovo ruolo del database denominato `Sales`. Viene aggiunto un utente del database denominato Barry all'appartenenza e viene illustrato come rimuovere il membro Barry.   Questo esempio può essere eseguito nel database di esempio [AdventureWorks](https://msftdbprodsamples.codeplex.com/).
  
```sql  
CREATE ROLE Sales;  
ALTER ROLE Sales ADD MEMBER Barry;  
ALTER ROLE Sales DROP MEMBER Barry;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-role-transact-sql.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [DROP ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-role-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sys.database_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)  
  
  
