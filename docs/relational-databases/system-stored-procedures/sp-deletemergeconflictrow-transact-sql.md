---
description: sp_deletemergeconflictrow (Transact-SQL)
title: sp_deletemergeconflictrow (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_deletemergeconflictrow
- sp_deletemergeconflictrow_TSQL
helpviewer_keywords:
- sp_deletemergeconflictrow
ms.assetid: 64cf1186-28b8-4cd9-88f1-a7808a9c8d60
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 392d64a854db224542438f28d821a3ade597bdc8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203687"
---
# <a name="sp_deletemergeconflictrow-transact-sql"></a>sp_deletemergeconflictrow (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elimina le righe da una tabella dei conflitti o dalla [MSmerge_conflicts_info &#40;tabella&#41;Transact-SQL ](../../relational-databases/system-tables/msmerge-conflicts-info-transact-sql.md) . Questa stored procedure viene eseguita nella stessa posizione di archiviazione della tabella con conflitti in qualsiasi database.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_deletemergeconflictrow [ [ @conflict_table = ] 'conflict_table' ]  
    [ , [ @source_object = ] 'source_object' ]  
    { , [ @rowguid = ] 'rowguid'  
        , [ @origin_datasource = ] 'origin_datasource' ] }  
    [ , [ @drop_table_if_empty = ] 'drop_table_if_empty' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @conflict_table = ] 'conflict_table'` Nome della tabella dei conflitti. *conflict_table* è di **tipo sysname** e il valore predefinito è **%** . Se il *conflict_table* viene specificato come null o **%** , si presuppone che si verifichi un conflitto di eliminazione e che la riga corrispondente a *rowguid* e *origin_datasource* e *Source_object* venga eliminata dalla tabella [MSmerge_conflicts_info &#40;Transact-SQL&#41;](../../relational-databases/system-tables/msmerge-conflicts-info-transact-sql.md) .  
  
`[ @source_object = ] 'source_object'` Nome della tabella di origine. *source_object* è di **tipo nvarchar (386)** e il valore predefinito è null.  
  
`[ @rowguid = ] 'rowguid'` Identificatore di riga per il conflitto di eliminazione. *rowguid* è di tipo **uniqueidentifier** e non prevede alcun valore predefinito.  
  
`[ @origin_datasource = ] 'origin_datasource'` Origine del conflitto. *origin_datasource* è di tipo **varchar (255)** e non prevede alcun valore predefinito.  
  
`[ @drop_table_if_empty = ] 'drop_table_if_empty'` Flag che indica che la *conflict_table* deve essere eliminata se è vuota. *drop_table_if_empty* è di tipo **varchar (10)** e il valore predefinito è false.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_deletemergeconflictrow** viene utilizzata nella replica di tipo merge.  
  
 [MSmerge_conflicts_info &#40;tabella&#41;Transact-SQL ](../../relational-databases/system-tables/msmerge-conflicts-info-transact-sql.md) è una tabella di sistema e non viene eliminata dal database, anche se è vuota.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_deletemergeconflictrow**.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
