---
title: Visualizzare informazioni sullo spazio allocato ai dati e ai log per un database
description: Informazioni su come visualizzare le informazioni sui dati e lo spazio di log per un database in SQL Server usando SQL Server Management Studio o Transact-SQL.
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- logs [SQL Server], space
- status information [SQL Server], space
- displaying space information
- disk space [SQL Server], displaying
- databases [SQL Server], space used
- viewing space information
- space allocation [SQL Server], displaying
- data space [SQL Server]
ms.assetid: c7b99463-4bab-4e9b-9217-fcb0898dc757
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 5c554ed600ce2317291cc01652bba07ad3d54d90
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748195"
---
# <a name="display-data-and-log-space-information-for-a-database"></a>Visualizzare le informazioni sullo spazio allocato ai dati e ai log per un database
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  In questo argomento si illustra come visualizzare le informazioni sullo spazio allocato ai dati e ai log per un database in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  

  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 L'autorizzazione per eseguire **sp_spaceused** è concessa al ruolo **public** . Solo i membri del ruolo predefinito del database **db_owner** possono specificare il parametro **\@updateusage**.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-display-data-and-log-space-information-for-a-database"></a>Per visualizzare le informazioni sullo spazio allocato ai dati e ai log per un database  
  
1. In Esplora oggetti connettersi a un'istanza del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , quindi espanderla.  
  
2. Espandere **Database**.  
  
3. Fare clic con il pulsante destro del mouse su un database, scegliere **Report**, **Report standard**, quindi fare clic su **Utilizzo disco**.  

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL

#### <a name="to-display-data-and-log-space-information-for-a-database-by-using-sp_spaceused"></a>Per visualizzare le informazioni sullo spazio allocato ai dati e ai log per un database utilizzando sp_spaceused
  
1. Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2. Dalla barra Standard fare clic su **Nuova query**.  
  
3. Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. In questo esempio viene utilizzata la stored procedure di sistema [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md) per segnalare le informazioni sullo spazio su disco per l'intero database, ovvero le tabelle e gli indici.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_spaceused;  
GO  
```  

#### <a name="to-display-data-space-used-by-object-and-allocation-unit-for-a-database"></a>Per visualizzare lo spazio dati utilizzato dall'oggetto e dall'unità di allocazione per un database
  
1. Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2. Dalla barra Standard fare clic su **Nuova query**.  
  
3. Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. In questo esempio vengono eseguite query [sulle viste del catalogo oggetti](../system-catalog-views/object-catalog-views-transact-sql.md) per segnalare l'utilizzo dello spazio su disco per ogni tabella e all'interno di ogni tabella per ogni [unità](../pages-and-extents-architecture-guide.md#IAM)  
  
```sql  
SELECT
  t.object_id,
  OBJECT_NAME(t.object_id) ObjectName,
  sum(u.total_pages) * 8 Total_Reserved_kb,
  sum(u.used_pages) * 8 Used_Space_kb,
  u.type_desc,
  max(p.rows) RowsCount
FROM
  sys.allocation_units u
  join sys.partitions p on u.container_id = p.hobt_id
  join sys.tables t on p.object_id = t.object_id
GROUP BY
  t.object_id,
  OBJECT_NAME(t.object_id),
  u.type_desc
ORDER BY
  Used_Space_kb desc,
  ObjectName
```  

#### <a name="to-display-data-and-log-space-information-for-a-database-by-querying-sysdatabase_files"></a>Per visualizzare le informazioni sullo spazio allocato ai dati e ai log per un database eseguendo una query su sys.database_files  
  
1. Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2. Dalla barra Standard fare clic su **Nuova query**.  
  
3. Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. In questo esempio si esegue una query sulla vista del catalogo [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md) per restituire informazioni specifiche sui file di dati e di log nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT file_id, name, type_desc, physical_name, size, max_size  
FROM sys.database_files ;  
GO  
  
```  
  
## <a name="see-also"></a>Vedere anche

 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sp_spaceused &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)   
 [Aggiungere file di dati o file di log a un database](../../relational-databases/databases/add-data-or-log-files-to-a-database.md)   
 [Eliminare file di dati o file di log da un database](../../relational-databases/databases/delete-data-or-log-files-from-a-database.md)  
