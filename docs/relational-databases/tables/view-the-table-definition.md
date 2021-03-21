---
description: Visualizzare la definizione di una tabella
title: Visualizzare la definizione di una tabella
ms.custom: ''
ms.date: 03/09/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- showing table properties
- displaying table properties
- tables [SQL Server], properties
- viewing table properties
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0ffdea67493c7d14255cfc543b1a8518bb3a8b4d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748891"
---
# <a name="view-the-table-definition"></a>Visualizzare la definizione di una tabella
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  È possibile visualizzare proprietà per una tabella in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Sicurezza](#Security)  
  
-   **Per visualizzare le proprietà di tabella:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È possibile vedere solo proprietà in una tabella se si possiede la tabella o sono state concesse autorizzazioni a quella tabella.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
  
#### <a name="to-show-table-properties-in-the-properties-window"></a>Per visualizzare le proprietà di una tabella nella finestra Proprietà  
  
1.  In Esplora oggetti selezionare la tabella per la quale si desidera mostrare le proprietà.  
  
2.  Fare clic con il pulsante destro del mouse sulla tabella, quindi scegliere **Proprietà** dal menu di scelta rapida. Per altre informazioni, vedere [Proprietà tabella](../../relational-databases/tables/table-properties-ssms.md).  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-show-table-properties"></a>Per mostrare le proprietà di tabella  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. Nell'esempio viene eseguito il stored procedure di sistema sp_help per restituire tutte le informazioni sulla colonna per l'oggetto specificato.  
  
```sql  
EXEC sp_help 'dbo.mytable';
```  
    
 Per ulteriori informazioni, vedere [sp_help (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md).

 In alternativa, è possibile eseguire una query sulle viste del catalogo di sistema direttamente per eseguire query sui metadati degli oggetti relativi a tabelle, schemi e colonne. Ad esempio:  
  
```sql
SELECT s.name, t.name, c.* FROM sys.columns AS c
INNER JOIN sys.tables AS t ON t.object_id = c.object_id
INNER JOIN sys.schemas AS s ON s.schema_id = t.schema_id
WHERE t.object_id = object_id('mytable') AND s.name = 'dbo';
```
    
 Per altre informazioni, vedere: 

* [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)    
* [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)    
* [sys.schemas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/schemas-catalog-views-sys-schemas.md)     

