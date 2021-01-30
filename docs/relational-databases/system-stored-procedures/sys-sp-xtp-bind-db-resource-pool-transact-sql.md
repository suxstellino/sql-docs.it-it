---
description: sys.sp_xtp_bind_db_resource_pool (Transact-SQL)
title: sys.sp_xtp_bind_db_resource_pool (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_xtp_bind_db_resource_pool_TSQL
- sp_xtp_bind_db_resource_pool
- sys.sp_xtp_bind_db_resource_pool_TSQL
- sys.sp_xtp_bind_db_resource_pool
dev_langs:
- TSQL
helpviewer_keywords:
- sp_xtp_bind_db_resource_pool
- sys.sp_xtp_bind_db_resource_pool
ms.assetid: c2a78073-626b-4159-996e-1808f6bfb6d2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 73312fdb49f223f8a275ade2a13bf2d988649404
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205090"
---
# <a name="syssp_xtp_bind_db_resource_pool-transact-sql"></a>sys.sp_xtp_bind_db_resource_pool (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Associa il database [!INCLUDE[hek_2](../../includes/hek-2-md.md)] specificato al pool di risorse specificato. Sia il database sia il pool di risorse devono essere disponibili prima di eseguire `sys.sp_xtp_bind_db_resource_pool`.  
  
 Tramite questa procedura di sistema viene creata un'associazione tra il pool di Resource Governor identificato da resource_pool_name e il database identificato da database_name. Non è necessario che gli oggetti del database siano tutti ottimizzati per la memoria al momento dell'associazione. In assenza di questi oggetti, non viene prelevata alcuna memoria dal pool di risorse. Questa associazione verrà utilizzata da Resource Governor per gestire la memoria allocata dagli allocatori di [!INCLUDE[hek_2](../../includes/hek-2-md.md)] come descritto di seguito.  
  
 Se è già disponibile un'associazione per un database specificato, viene restituito un errore dalla procedura.  In nessun caso un database può disporre di più associazioni attive.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
sys.sp_xtp_bind_db_resource_pool 'database_name', 'resource_pool_name'  
```  
  
## <a name="arguments"></a>Argomenti  
 database_name  
 Nome di un database abilitato per [!INCLUDE[hek_2](../../includes/hek-2-md.md)] esistente.  
  
 resource_pool_name  
 Nome di un pool di risorse esistente.  
  
## <a name="messages"></a>Messaggi  
 In caso di errore, tramite `sp_xtp_bind_db_resource_pool` viene restituito uno di questi messaggi.  
  
 **Il database non esiste**  
 Database_name deve fare riferimento a un database esistente. Se non è disponibile alcun database con l'ID specificato, viene restituito il messaggio seguente:   
*L'ID di database% d non esiste.  Usare un ID di database valido per questa associazione.*  
  
```  
Msg 911, Level 16, State 18, Procedure sp_xtp_bind_db_resource_pool_internal, Line 51  
Database 'Hekaton_DB213' does not exist. Make sure that the name is entered correctly.  
```  
  
**Il database è un database di sistema**  
 Le tabelle di [!INCLUDE[hek_2](../../includes/hek-2-md.md)] non possono essere create in database di sistema.  Pertanto, non è possibile creare un'associazione di memoria di [!INCLUDE[hek_2](../../includes/hek-2-md.md)] per un database di questo tipo.  Viene restituito l'errore seguente:  
*Database_name% s fa riferimento a un database di sistema.  I pool di risorse possono essere associati solo a un database utente.*  
  
```  
Msg 41371, Level 16, State 1, Procedure sp_xtp_bind_db_resource_pool_internal, Line 51  
Binding to a resource pool is not supported for system database 'master'. This operation can only be performed on a user database.  
```  
  
**Il pool di risorse non esiste**  
 Il pool di risorse identificato da resource_pool_name deve essere disponibile prima di eseguire `sp_xtp_bind_db_resource_pool`.  Se non è disponibile alcun pool con l'ID specificato, viene restituito l'errore seguente:  
*Il pool di risorse% s non esiste.  Immettere un nome valido per il pool di risorse.*  
  
```  
Msg 41370, Level 16, State 1, Procedure sp_xtp_bind_db_resource_pool_internal, Line 51  
Resource pool 'Pool_Hekaton' does not exist or resource governor has not been reconfigured.  
```  
  
**Pool_name fa riferimento a un pool di sistema riservato**  
 I nomi dei pool "INTERNAL" e "DEFAULT" sono riservati per i pool di sistema.  Non è possibile associare in modo esplicito un database a uno di questi.  Se viene immesso un nome di pool di sistema, viene restituito l'errore seguente:  
*Il pool di risorse% s è un pool di risorse di sistema.  I pool di risorse di sistema non possono essere associati in modo esplicito a un database utilizzando questa procedura.*  
  
```  
Msg 41373, Level 16, State 1, Procedure sp_xtp_bind_db_resource_pool_internal, Line 51  
Database 'Hekaton_DB' cannot be explicitly bound to the resource pool 'internal'. A database can only be bound only to a user resource pool.  
```  
  
**Il database è già associato a un altro pool di risorse**  
 Un database può essere associato a un solo pool di risorse in qualsiasi momento. Le associazioni di database ai pool di risorse devono essere rimosse in modo esplicito prima che possano essere associate a un altro pool. Vedere [sys.sp_xtp_unbind_db_resource_pool &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-xtp-unbind-db-resource-pool-transact-sql.md).  
*Il database% s è già associato al pool di risorse% s.  Prima di poter creare una nuova associazione, è necessario annullare l'associazione.*  
  
```  
Msg 41372, Level 16, State 1, Procedure sp_xtp_bind_db_resource_pool_internal, Line 54  
Database 'Hekaton_DB' is currently bound to a resource pool. A database must be unbound before creating a new binding.  
```  
  
 Quando l'operazione viene completata, tramite `sp_xtp_bind_db_resource_pool` viene restituito il messaggio riportato di seguito.  
  
**Associazione completata**  
 Quando l'operazione viene completata, tramite la funzione viene restituito il seguente messaggio di operazione riuscita, che viene registrato in SQL ERRORLOG  
*Associazione di risorse creata correttamente tra il database con ID %d e il pool di risorse con ID %d.*  
  
## <a name="examples"></a>Esempi  
R.  Nell'esempio di codice seguente viene associato il database Hekaton_DB al pool di risorse Pool_Hekaton.  
  
```sql  
sys.sp_xtp_bind_db_resource_pool N'Hekaton_DB', N'Pool_Hekaton'  
```  
 
 L'associazione viene applicata alla successiva connessione del database.  
 
 B. Esempio esteso dell'esempio precedente che include alcuni controlli di base.  Eseguire il comando seguente [!INCLUDE[tsql](../../includes/tsql-md.md)] in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]\:
 
```sql
DECLARE @resourcePool sysname = N'Pool_Hekaton';
DECLARE @database sysname = N'Hekaton_DB';

-- Check whether resource pool exists
IF NOT EXISTS (
    SELECT * FROM sys.resource_governor_resource_pools WHERE name = @resourcePool
    )
BEGIN
    SELECT N'Resource pool "' + @resourcePool + N'" does not exist or resource governor has not been reconfigured.';
END
-- Check whether database is already bound to a resource pool
ELSE IF EXISTS (
    SELECT p.name
    FROM sys.databases d
    JOIN sys.resource_governor_resource_pools p
    ON d.resource_pool_id = p.pool_id
    WHERE d.name = @database
    )
BEGIN
    SELECT N'Database "' + @database + N'" is currently bound to resource pool "' + @resourcePool  + N'". A database must be unbound before creating a new binding.';
END
-- Bind resource pool to database.
ELSE BEGIN
    EXEC sp_xtp_bind_db_resource_pool @database, @resourcePool; 
END 
``` 
  
## <a name="requirements"></a>Requisiti  
  
-   Sia il database specificato da `database_name` sia il pool di risorse specificato da `resource_pool_name` devono essere disponibili prima di poter essere associati.  
  
-   È richiesta l'autorizzazione CONTROL SERVER.  
  
## <a name="see-also"></a>Vedere anche  
 [a un pool di risorse, vedere l'argomento](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md)   
 [sys.sp_xtp_unbind_db_resource_pool &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-xtp-unbind-db-resource-pool-transact-sql.md)  
  
  
