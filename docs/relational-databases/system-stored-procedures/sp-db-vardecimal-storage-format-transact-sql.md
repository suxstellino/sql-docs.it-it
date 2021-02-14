---
description: sp_db_vardecimal_storage_format (Transact-SQL)
title: sp_db_vardecimal_storage_format (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_db_vardecimal_storage_format
- sp_db_vardecimal_storage_format_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_db_vardecimal_storage_format
- decimal data type, compressing
- compressing decimal data
- numeric data type, compressing
- database compression [SQL Server]
- table compression [SQL Server]
ms.assetid: 9920b2f7-b802-4003-913c-978c17ae4542
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c5481a80e57a60180112145e87c64c9f383ed473
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342756"
---
# <a name="sp_db_vardecimal_storage_format-transact-sql"></a>sp_db_vardecimal_storage_format (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce lo stato corrente del formato di archiviazione vardecimal di un database oppure abilita il formato di archiviazione vardecimal per un database.  A partire da [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], i database utente sono sempre abilitati. L'abilitazione del formato di archiviazione vardecimal per i database è necessaria solo in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].  
  
> [!NOTE]  
> In [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] è supportato il formato di archiviazione vardecimal. Tuttavia, poiché la compressione a livello di riga consente di ottenere gli stessi risultati, tale formato viene deprecato. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]
  
> [!IMPORTANT]  
> La modifica dello stato del formato di archiviazione vardecimal di un database può influire su backup e recupero, mirroring del database, sp_attach_db, log shipping e replica.  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
sp_db_vardecimal_storage_format [ [ @dbname = ] 'database_name']   
    [ , [ @vardecimal_storage_format = ] { 'ON' | 'OFF' } ]   
[;]  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @dbname =]'*database_name*'  
 Nome del database per il quale deve essere modificato il formato di archiviazione. *database_name* è di **tipo sysname** e non prevede alcun valore predefinito. Se il nome del database viene omesso, viene restituito lo stato del formato di archiviazione vardecimal di tutti i database nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [ @vardecimal_storage_format =] {' On ' |' OFF '}  
 Specifica se il formato di archiviazione vardecimal è abilitato. @vardecimal_storage_format può essere ON oppure OFF. Il parametro è di tipo **varchar (3)** e non prevede alcun valore predefinito. Se viene specificato il nome di un database ma viene omesso @vardecimal_storage_format, viene restituita l'impostazione corrente del database specificato. 
 
 > [!IMPORTANT]
 > Questo argomento non ha effetto in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] o versioni successive.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 Se il formato di archiviazione del database non può essere modificato, sp_db_vardecimal_storage_format restituisce un errore. Se lo stato corrente del database corrisponde a quello specificato, la stored procedure non produce alcun effetto.  
  
 Se l' @vardecimal_storage_format argomento non viene specificato, restituisce il nome del database Columns e lo stato vardecimal.  
  
## <a name="remarks"></a>Commenti  
 sp_db_vardecimal_storage_format restituisce lo stato di vardecimal ma non consente di modificare tale stato.  
  
 sp_db_vardecimal_storage_format avrà esito negativo nelle circostanze seguenti:  
  
-   Non sono presenti utenti attivi nel database.  
  
-   È abilitato il mirroring del database.  
  
-   L'edizione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supporta il formato di archiviazione vardecimal.  
  
 Per modificare lo stato del formato di archiviazione vardecimal in OFF, è necessario che un database sia impostato sulla modalità di recupero con registrazione minima. In caso di impostazione di un database su tale modalità, la catena di log è interrotta. Dopo aver impostato lo stato del formato di archiviazione vardecimal su OFF, eseguire un backup completo del database.  
  
 La modifica dello stato in OFF avrà esito negativo se sono presenti tabelle in cui viene utilizzata la compressione di database di tipo vardecimal. Per modificare il formato di archiviazione di una tabella, utilizzare [sp_tableoption](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md). Per determinare le tabelle di un database in cui viene utilizzato il formato di archiviazione vardecimal, utilizzare la funzione `OBJECTPROPERTY` e cercare la proprietà `TableHasVarDecimalStorageFormat`, come illustrato nell'esempio seguente.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
SELECT name, object_id, type_desc  
FROM sys.objects   
 WHERE OBJECTPROPERTY(object_id,   
   N'TableHasVarDecimalStorageFormat') = 1 ;  
GO  
```  
  
## <a name="examples"></a>Esempio  
 Tramite il codice seguente viene abilitata la compressione nel database `AdventureWorks2012`, viene verificato lo stato e quindi vengono compresse le colonne decimal e numeric della tabella `Sales.SalesOrderDetail`.  
  
```sql  
USE master ;  
GO  
  
EXEC sp_db_vardecimal_storage_format 'AdventureWorks2012', 'ON' ;  
GO  
  
-- Check the vardecimal storage format state for  
-- all databases in the instance.  
EXEC sp_db_vardecimal_storage_format ;  
GO  
  
USE AdventureWorks2012 ;  
GO  
  
EXEC sp_tableoption 'Sales.SalesOrderDetail', 'vardecimal storage format', 1 ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)  
  
  
