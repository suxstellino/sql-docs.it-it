---
description: sp_replcmds (Transact-SQL)
title: sp_replcmds (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replcmds_TSQL
- sp_replcmds
helpviewer_keywords:
- sp_replcmds
ms.assetid: 7e932f80-cc6e-4109-8db4-2b7c8828df73
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d8d61544e171e102c04f829ab7c302048c930d21
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193520"
---
# <a name="sp_replcmds-transact-sql"></a>sp_replcmds (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Restituisce i comandi per le transazioni contrassegnate per la replica. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
> [!IMPORTANT]  
>  È consigliabile eseguire la procedura **sp_replcmds** solo per risolvere i problemi relativi alla replica.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_replcmds [ @maxtrans = ] maxtrans  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @maxtrans = ] maxtrans` Numero di transazioni per cui restituire informazioni. *maxtrans* è di **tipo int** e il valore predefinito è **1**, che specifica la transazione successiva in attesa di distribuzione.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**ID articolo**|**int**|ID dell'articolo.|  
|**partial_command**|**bit**|Indica se si tratta di un comando parziale.|  
|**command**|**varbinary (1024)**|Valore del comando.|  
|**xactid**|**binary(10)**|ID della transazione.|  
|**xact_seqno**|**varbinary(16)**|Numero di sequenza della transazione.|  
|**publication_id**|**int**|ID della pubblicazione.|  
|**command_id**|**int**|ID del comando in [MSrepl_commands](../../relational-databases/system-tables/msrepl-commands-transact-sql.md).|  
|**command_type**|**int**|Tipo di comando.|  
|**originator_srvname**|**sysname**|Server in cui ha origine la transazione.|  
|**originator_db**|**sysname**|Database in cui ha origine la transazione.|  
|**pkHash**|**int**|Solo per uso interno.|  
|**originator_publication_id**|**int**|ID della pubblicazione in cui ha origine la transazione.|  
|**originator_db_version**|**int**|Versione del database in cui ha origine la transazione.|  
|**originator_lsn**|**varbinary(16)**|Identifica il numero di sequenza del file di log (LSN) per il comando nella pubblicazione di origine.|  
  
## <a name="remarks"></a>Commenti  
 **sp_replcmds** viene utilizzato dal processo di lettura log nella replica transazionale.  
  
 La replica considera il primo client che esegue **sp_replcmds** all'interno di un determinato database come lettura log.  
  
 Questa procedura può generare comandi per tabelle qualificate con il nome del proprietario oppure non qualifica il nome della tabella (impostazione predefinita). L'aggiunta di nomi di tabella qualificati consente di replicare i dati di tabelle di proprietà di un utente specifico di un database in tabelle di proprietà dello stesso utente in un altro database.  
  
> [!NOTE]  
>  Poiché il nome di tabella nel database di origine è qualificato dal nome del proprietario, per la tabella del database di destinazione è necessario specificare lo stesso nome di proprietario.  
  
 I client che tentano di eseguire **sp_replcmds** nello stesso database ricevono l'errore 18752 fino a quando il primo client si disconnette. Quando il primo client si disconnette, un altro client può eseguire **sp_replcmds** e diventa la nuova lettura log.  
  
 Viene aggiunto un messaggio di avviso numero 18759 al [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] log degli errori di e al [!INCLUDE[msCoName](../../includes/msconame-md.md)] registro delle applicazioni di Windows se **sp_replcmds** non è in grado di replicare un comando di testo perché il puntatore di testo non è stato recuperato nella stessa transazione.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_replcmds**.  
  
## <a name="see-also"></a>Vedere anche  
 [Messaggi di errore](../../relational-databases/native-client-odbc-error-messages/error-messages.md)   
 [sp_repldone &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-repldone-transact-sql.md)   
 [sp_replflush &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-replflush-transact-sql.md)   
 [sp_repltrans &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-repltrans-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
