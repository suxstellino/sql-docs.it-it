---
description: sp_browsereplcmds (Transact-SQL)
title: sp_browsereplcmds (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_browsereplcmds_TSQL
- sp_browsereplcmds
helpviewer_keywords:
- sp_browsereplcmds
ms.assetid: 30abcb41-1d18-4f43-a692-4c80914c0450
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f798dcb9689221d9a8ef1964d4237a1fe649a4c5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206627"
---
# <a name="sp_browsereplcmds-transact-sql"></a>sp_browsereplcmds (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Restituisce un set di risultati in una versione leggibile dei comandi replicati archiviati nel database di distribuzione e viene utilizzata come strumento di diagnostica. La stored procedure viene eseguita nel database di distribuzione del server di distribuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_browsereplcmds [ [ @xact_seqno_start = ] 'xact_seqno_start' ]  
    [ , [ @xact_seqno_end = ] 'xact_seqno_end' ]   
    [ , [ @originator_id = ] 'originator_id' ]  
    [ , [ @publisher_database_id = ] 'publisher_database_id' ]  
    [ , [ @article_id = ] 'article_id' ]  
    [ , [ @command_id= ] command_id ]  
    [ , [ @agent_id = ] agent_id ]  
    [ , [ @compatibility_level = ] compatibility_level ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @xact_seqno_start = ] 'xact_seqno_start'` Specifica il numero di sequenza esatto con un valore più basso da restituire. *xact_seqno_start* è di **nchar (22)** e il valore predefinito è è 0x00000000000000000000.  
  
`[ @xact_seqno_end = ] 'xact_seqno_end'` Specifica il numero di sequenza esatto più alto da restituire. *xact_seqno_end* è di **nchar (22)** e il valore predefinito è 0xFFFFFFFFFFFFFFFFFFFF.  
  
`[ @originator_id = ] 'originator_id'` Specifica se vengono restituiti i comandi con il *originator_id* specificato. *originator_id* è di **tipo int** e il valore predefinito è null.  
  
`[ @publisher_database_id = ] 'publisher_database_id'` Specifica se vengono restituiti i comandi con il *publisher_database_id* specificato. *publisher_database_id* è di **tipo int** e il valore predefinito è null.  
  
`[ @article_id = ] 'article_id'` Specifica se vengono restituiti i comandi con il *article_id* specificato. *article_id* è di **tipo int** e il valore predefinito è null.  
  
`[ @command_id = ] command_id` Posizione del comando in [MSrepl_commands &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/msrepl-commands-transact-sql.md) da decodificare. *command_id* è di **tipo int** e il valore predefinito è null. Se specificato, è necessario specificare anche tutti gli altri parametri e *xact_seqno_start* devono essere identici a *xact_seqno_end*.  
  
`[ @agent_id = ] agent_id` Specifica che vengono restituiti solo i comandi per un agente di replica specifico. *agent_id* è di **tipo int** e il valore predefinito è null.  
  
`[ @compatibility_level = ] compatibility_level` Versione di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui il *COMPATIBILITY_LEVEL* è di **tipo int** e il valore predefinito è 9 milioni.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**xact_seqno**|**varbinary(16)**|Numero di sequenza del comando.|  
|**originator_srvname**|**sysname**|Server in cui ha origine la transazione.|  
|**originator_db**|**sysname**|Database in cui ha origine la transazione.|  
|**article_id**|**int**|ID dell'articolo.|  
|**type**|**int**|Tipo di comando.|  
|**partial_command**|**bit**|Indica se si tratta di un comando parziale.|  
|**hashkey**|**int**|Solo per uso interno.|  
|**originator_publication_id**|**int**|ID della pubblicazione in cui ha origine la transazione.|  
|**originator_db_version**|**int**|Versione del database in cui ha origine la transazione.|  
|**originator_lsn**|**varbinary(16)**|Identifica il numero di sequenza del file di log (LSN) per il comando nella pubblicazione di origine. Utilizzato nella replica transazionale peer-to-peer.|  
|**command**|**nvarchar(1024)**|Comando [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
|**command_id**|**int**|ID del comando in [MSrepl_commands](../../relational-databases/system-tables/msrepl-commands-transact-sql.md).|  
  
 È possibile suddividere i comandi lunghi in più righe nei set di risultati.  
  
## <a name="remarks"></a>Commenti  
 **sp_browsereplcmds** viene utilizzata nella replica transazionale.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o i membri dei ruoli predefiniti del database **db_owner** o **replmonitor** nel database di distribuzione possono eseguire **sp_browsereplcmds**.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_replcmds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md)   
 [sp_replshowcmds &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-replshowcmds-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
