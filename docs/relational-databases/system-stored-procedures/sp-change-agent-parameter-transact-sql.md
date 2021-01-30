---
description: sp_change_agent_parameter (Transact-SQL)
title: sp_change_agent_parameter (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_change_agent_parameter_TSQL
- sp_change_agent_parameter
helpviewer_keywords:
- sp_change_agent_parameter
ms.assetid: f1fbecc7-e64f-405c-8067-6b38c1f3c0a0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e8bfb83ad0b05f57d4be97838882288937ffbcc1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206595"
---
# <a name="sp_change_agent_parameter-transact-sql"></a>sp_change_agent_parameter (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifica un parametro di un profilo dell'agente di replica archiviato nella tabella di sistema [MSagent_parameters](../../relational-databases/system-tables/msagent-parameters-transact-sql.md) . Questa stored procedure viene eseguita in qualsiasi database del server di distribuzione in cui l'agente è in esecuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_change_agent_parameter [ @profile_id= ] profile_id, [ @parameter_name= ] 'parameter_name', [ @parameter_value= ] 'parameter_value'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @profile_id = ] profile_id,` ID del profilo. *profile_id* è di **tipo int** e non prevede alcun valore predefinito.  
  
`[ @parameter_name = ] 'parameter_name'` Nome del parametro. *parameter_name* è di **tipo sysname** e non prevede alcun valore predefinito. I parametri che è possibile modificare per i profili sistema dipendono dal tipo di agente. Per individuare il tipo di agente rappresentato da questo *profile_id* , individuare la colonna *profile_id* nella tabella **MSagent_profiles** e prendere nota del valore di *agent_type* .  
  
> [!NOTE]  
>  Se un parametro è supportato per un *agent_type* specificato, ma non è stato definito nel profilo agente, viene restituito un errore. Per aggiungere un parametro a un profilo agente, è necessario eseguire [sp_add_agent_parameter](../../relational-databases/system-stored-procedures/sp-add-agent-parameter-transact-sql.md).  
  
 Per un agente di snapshot (*agent_type* = **1**), se definito nel profilo, è possibile modificare le proprietà seguenti:  
  
-   **70Subscribers**  
  
-   **BcpBatchSize**  
  
-   **HistoryVerboseLevel più elevato**  
  
-   **LoginTimeout**  
  
-   **MaxBcpThreads**  
  
-   **MaxNetworkOptimization**  
  
-   **Output**  
  
-   **OutputVerboseLevel**  
  
-   **PacketSize**  
  
-   **QueryTimeout**  
  
-   **StartQueueTimeout**  
  
-   **UsePerArticleContentsView**  
  
 Per un agente di lettura log (*agent_type* = **2**), se definito nel profilo, è possibile modificare le proprietà seguenti:  
  
-   **HistoryVerboseLevel più elevato**  
  
-   **LoginTimeout**  
  
-   **MessageInterval**  
  
-   **Output**  
  
-   **OutputVerboseLevel**  
  
-   **PacketSize**  
  
-   **PollingInterval**  
  
-   **QueryTimeout**  
  
-   **ReadBatchSize**  
  
-   **ReadBatchThreshold**  
  
 Per un agente di distribuzione (*agent_type* = **3**), se definito nel profilo, è possibile modificare le proprietà seguenti:  
  
-   **BcpBatchSize**  
  
-   **CommitBatchSize**  
  
-   **CommitBatchThreshold**  
  
-   **FileTransferType**  
  
-   **HistoryVerboseLevel più elevato**  
  
-   **KeepAliveMessageInterval**  
  
-   **LoginTimeout**  
  
-   **MaxBcpThreads**  
  
-   **MaxDeliveredTransactions**  
  
-   **MessageInterval**  
  
-   **Output**  
  
-   **OutputVerboseLevel**  
  
-   **PacketSize**  
  
-   **PollingInterval**  
  
-   **QueryTimeout**  
  
-   **QuotedIdentifier**  
  
-   **SkipErrors**  
  
-   **TransactionsPerHistory**  
  
 Per un agente di merge (*agent_type* = **4**), se definito nel profilo, è possibile modificare le proprietà seguenti:  
  
-   **AltSnapshotFolder**  
  
-   **BcpBatchSize**  
  
-   **ChangesPerHistory**  
  
-   **DestThreads**  
  
-   **DownloadGenerationsPerBatch**  
  
-   **DownloadReadChangesPerBatch**  
  
-   **DownloadWriteChangesPerBatch**  
  
-   **DynamicSnapshotLocation**  
  
-   **ExchangeType**  
  
-   **FastRowCount**  
  
-   **FileTransferType**  
  
-   **GenerationChangeThreshold**  
  
-   **HistoryVerboseLevel più elevato**  
  
-   **InputMessageFile**  
  
-   **InteractiveResolution**  
  
-   **InterruptOnMessagePattern**  
  
-   **KeepAliveMessageInterval**  
  
-   **LoginTimeout**  
  
-   **MaxBcpThreads**  
  
-   **MaxDownloadChanges**  
  
-   **MaxUploadChanges**  
  
-   **MetadataRetentionCleanup**  
  
-   **NumDeadlockRetries**  
  
-   **Output**  
  
-   **OutputMessageFile**  
  
-   **OutputVerboseLevel**  
  
-   **PacketSize**  
  
-   **ParallelUploadDownload**  
  
-   **PauseOnMessagePattern**  
  
-   **PauseTime**  
  
-   **PollingInterval**  
  
-   **ProcessMessagesAtPublisher**  
  
-   **ProcessMessagesAtSubscriber**  
  
-   **QueryTimeout**  
  
-   **QueueSizeMultiplier**  
  
-   **SrcThreads**  
  
-   **StartQueueTimeout**  
  
-   **SyncToAlternate**  
  
-   **UploadGenerationsPerBatch**  
  
-   **Parametro UploadReadChangesPerBatch**  
  
-   **UploadWriteChangesPerBatch**  
  
-   **UseInprocLoader**  
  
-   **Convalida**  
  
-   **ValidateInterval**  
  
 Per un agente di lettura coda (*agent_type* = **9**), se definito nel profilo, è possibile modificare le proprietà seguenti:  
  
-   **HistoryVerboseLevel più elevato**  
  
-   **LoginTimeout**  
  
-   **Output**  
  
-   **OutputVerboseLevel**  
  
-   **PollingInterval**  
  
-   **QueryTimeout**  
  
-   **ResolverState**  
  
-   **SQLQueueMode**  
  
 Per visualizzare i parametri definiti per un determinato profilo, eseguire **sp_help_agent_profile** e prendere nota del *profile_name* associato al *profile_id*. Con il *profile_id* appropriato, eseguire successivamente **sp_help_agent_parameters** utilizzando tale *profile_id* per visualizzare i parametri associati al profilo. I parametri possono essere aggiunti a un profilo eseguendo [sp_add_agent_parameter](../../relational-databases/system-stored-procedures/sp-add-agent-parameter-transact-sql.md).  
  
`[ @parameter_value = ] 'parameter_value'` Nuovo valore del parametro. *parameter_value* è di **tipo nvarchar (255)** e non prevede alcun valore predefinito.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_change_agent_parameter** viene utilizzato in tutti i tipi di replica.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_change_agent_parameter**.  
  
## <a name="see-also"></a>Vedere anche  
 [Profili degli agenti di replica](../../relational-databases/replication/agents/replication-agent-profiles.md)   
 [Replication Distribution Agent](../../relational-databases/replication/agents/replication-distribution-agent.md)   
 [Replication Log Reader Agent](../../relational-databases/replication/agents/replication-log-reader-agent.md)   
 [Replication Merge Agent](../../relational-databases/replication/agents/replication-merge-agent.md)   
 [Agente di lettura coda repliche](../../relational-databases/replication/agents/replication-queue-reader-agent.md)   
 [Replication Snapshot Agent](../../relational-databases/replication/agents/replication-snapshot-agent.md)   
 [sp_add_agent_parameter &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-agent-parameter-transact-sql.md)   
 [sp_drop_agent_parameter &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-drop-agent-parameter-transact-sql.md)   
 [sp_help_agent_parameter &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-agent-parameter-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
