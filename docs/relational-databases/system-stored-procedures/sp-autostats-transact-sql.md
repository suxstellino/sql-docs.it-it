---
description: sp_autostats (Transact-SQL)
title: sp_autostats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_autostats_TSQL
- sp_autostats
dev_langs:
- TSQL
helpviewer_keywords:
- sp_autostats
ms.assetid: d1df8c15-ee73-49eb-9d13-6e98943c3e38
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4a1746acf81c59b5b83a45d1be0403d4d58ad6c0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199231"
---
# <a name="sp_autostats-transact-sql"></a>sp_autostats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Visualizza o modifica l'opzione di aggiornamento delle statistiche automatiche AUTO_UPDATE_STATISTICS per un indice, un oggetto statistiche, una tabella o una vista indicizzata.  
  
 Per ulteriori informazioni sull'opzione AUTO_UPDATE_STATISTICS, vedere [Opzioni ALTER DATABASE SET &#40;&#41;e statistiche Transact-SQL](../../t-sql/statements/alter-database-transact-sql-set-options.md) . [](../../relational-databases/statistics/statistics.md)  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_autostats [ @tblname = ] 'table_or_indexed_view_name'   
    [ , [ @flagc = ] 'stats_flag' ]   
    [ , [ @indname = ] 'statistics_name' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @tblname = ] 'table_or_indexed_view_name'` Nome della tabella o della vista indicizzata in cui visualizzare l'opzione AUTO_UPDATE_STATISTICS. *table_or_indexed_view_name* è di **tipo nvarchar (776)** e non prevede alcun valore predefinito.  
  
`[ @flagc = ] 'stats_flag'` Aggiorna l'opzione AUTO_UPDATE_STATISTICS a uno dei valori seguenti:  
  
 **on** = on  
  
 **disattivato** = disattivato  
  
 Se *stats_flag* non è specificato, visualizzare l'impostazione di AUTO_UPDATE_STATISTICS corrente. *stats_flag* è di tipo **varchar (10)** e il valore predefinito è null.  
  
`[ @indname = ] 'statistics_name'` Nome delle statistiche per cui visualizzare o aggiornare l'opzione AUTO_UPDATE_STATISTICS. Per visualizzare le statistiche per un indice, è possibile utilizzare il nome dell'indice, in quanto un indice e l'oggetto statistiche corrispondente hanno lo stesso nome.  
  
 *statistics_name* è di **tipo sysname** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 Se viene specificato *stats_flag* , **sp_autostats** segnala l'azione che è stata eseguita, ma non restituisce alcun set di risultati.  
  
 Se non si specifica *stats_flag* , **sp_autostats** restituisce il set di risultati seguente.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**Index Name**|**sysname**|Nome dell'indice o delle statistiche.|  
|**AUTOSTATS**|**varchar (3)**|Valore corrente dell'opzione AUTO_UPDATE_STATISTICS.|  
|**Ultimo aggiornamento**|**datetime**|Data dell'aggiornamento più recente delle statistiche.|  
  
 Il set di risultati per una tabella o una vista indicizzata include le statistiche create per gli indici, le statistiche a colonna singola generate con l'opzione AUTO_CREATE_STATISTICS e le statistiche create con l'istruzione [Create Statistics](../../t-sql/statements/create-statistics-transact-sql.md) .  
  
## <a name="remarks"></a>Commenti  
 Se l'indice specificato è disabilitato oppure la tabella specificata include un indice cluster disabilitato, viene visualizzato un messaggio di errore.  
  
 AUTO_UPDATE_STATISTICS è sempre OFF per le tabelle ottimizzate per la memoria.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per modificare l'opzione AUTO_UPDATE_STATISTICS è richiesta l'appartenenza al ruolo predefinito del database **db_owner** oppure l'autorizzazione ALTER per *table_name*. Per visualizzare l'opzione AUTO_UPDATE_STATISTICS è richiesta l'appartenenza al ruolo **public** .  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-display-the-status-of-all-statistics-on-a-table"></a>R. Visualizzazione dello stato di tutte le statistiche in una tabella  
 Nell'esempio seguente viene visualizzato lo stato di tutte le statistiche della tabella `Product`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_autostats 'Production.Product';  
GO  
```  
  
### <a name="b-enable-auto_update_statistics-for-all-statistics-on-a-table"></a>B. Abilitazione di AUTO_UPDATE_STATISTICS per tutte le statistiche di una tabella  
 In questo esempio viene abilitata l'opzione AUTO_UPDATE_STATISTICS per tutte le statistiche della tabella `Product`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_autostats 'Production.Product', 'ON';  
GO  
```  
  
### <a name="c-disable-auto_update_statistics-for-a-specific-index"></a>C. Disabilitazione di AUTO_UPDATE_STATISTICS per un indice specifico  
 Nell'esempio seguente l'opzione AUTO_UPDATE_STATISTICS viene disabilitata per l'indice `AK_Product_Name` della tabella `Product`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_autostats 'Production.Product', 'OFF', AK_Product_Name;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Statistiche](../../relational-databases/statistics/statistics.md)   
 [Opzioni ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)   
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [DROP STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [sp_createstats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-createstats-transact-sql.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
