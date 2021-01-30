---
description: sp_help_downloadlist (Transact-SQL)
title: sp_help_downloadlist (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_downloadlist_TSQL
- sp_help_downloadlist
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_downloadlist
ms.assetid: 745b265b-86e8-4399-b928-c6969ca1a2c8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6f60b3da9c05d38b60573aa272c33a7cb6915116
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208921"
---
# <a name="sp_help_downloadlist-transact-sql"></a>sp_help_downloadlist (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elenca tutte le righe nella tabella di sistema **sysdownloadlist** per il processo fornito o tutte le righe se non viene specificato alcun processo.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_help_downloadlist { [ @job_id = ] job_id | [ @job_name = ] 'job_name' }   
     [ , [ @operation = ] 'operation' ]   
     [ , [ @object_type = ] 'object_type' ]   
     [ , [ @object_name = ] 'object_name' ]   
     [ , [ @target_server = ] 'target_server' ]   
     [ , [ @has_error = ] has_error ]   
     [ , [ @status = ] status ]   
     [ , [ @date_posted = ] date_posted ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_id = ] job_id` Numero di identificazione del processo per cui restituire informazioni. *job_id* è di tipo **uniqueidentifier** e il valore predefinito è null.  
  
`[ @job_name = ] 'job_name'` Nome del processo. *job_name* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  È necessario specificare *job_id* o *job_name* , ma non è possibile specificarli entrambi.  
  
`[ @operation = ] 'operation'` Operazione valida per il processo specificato. *Operation* è di tipo **varchar (64)** e il valore predefinito è null. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**DIFETTO**|Operazione del server che richiede al server di destinazione di escludere dal servizio **SQLServerAgent** master.|  
|**DELETE**|Operazione del processo che rimuove un intero processo.|  
|**INSERT**|Operazione del processo che inserisce un intero processo o ne aggiorna uno esistente. Include tutti i passaggi e le pianificazioni del processo, se applicabile.|  
|**RE-ENLIST**|Operazione del server con cui viene attivato il rinvio delle informazioni di integrazione del server di destinazione, tra cui l'intervallo di polling e il fuso orario per il dominio multiserver. Il server di destinazione Riscarica anche i dettagli **MSXOperator** .|  
|**SET-POLL**|Operazione del server con cui viene impostato l'intervallo di tempo in secondi per il polling del dominio multiserver eseguito dai server di destinazione. Se specificato, il *valore* viene interpretato come il valore dell'intervallo necessario e può essere un valore compreso tra **10** e **28.800**.|  
|**INIZIARE**|Operazione del processo con cui viene richiesto l'avvio dell'esecuzione del processo.|  
|**ARRESTARE**|Operazione del processo con cui viene richiesto l'arresto dell'esecuzione del processo.|  
|**SYNC-TIME**|Operazione del server con cui viene attivata la sincronizzazione del clock di sistema dei server di destinazione con il clock di sistema del dominio multiserver. Si tratta di un'operazione onerosa ed è pertanto consigliabile non eseguirla di frequente.|  
|**UPDATE**|Operazione di processo che aggiorna solo le informazioni di **sysjobs** per un processo, non i passaggi o le pianificazioni del processo. Viene chiamato automaticamente da **sp_update_job**.|  
  
`[ @object_type = ] 'object_type'` Tipo di oggetto per il processo specificato. *object_type* è di tipo **varchar (64)** e il valore predefinito è null. *object_type* può essere un processo o un server. Per ulteriori informazioni sui valori di *object_type* validi, vedere [sp_add_category &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-category-transact-sql.md).  
  
`[ @object_name = ] 'object_name'` Nome dell'oggetto. *object_name* è di **tipo sysname** e il valore predefinito è null. Se *object_type* è job, *object_name* è il nome del processo. Se *object_type* è server, *object_name* è il nome del server.  
  
`[ @target_server = ] 'target_server'` Nome del server di destinazione. *target_server* è di **tipo nvarchar (128)** e il valore predefinito è null.  
  
`[ @has_error = ] has_error` Indica se il processo deve riconoscere gli errori. *has_error* è di **tinyint** e il valore predefinito è null, che indica che non deve essere riconosciuto alcun errore. **1** indica che tutti gli errori devono essere riconosciuti.  
  
`[ @status = ] status` Stato del processo. *status* è di **tinyint** e il valore predefinito è null.  
  
`[ @date_posted = ] date_posted` Data e ora per le quali è necessario includere nel set di risultati tutte le voci effettuate dopo la data e l'ora specificate. *date_posted* è di tipo **DateTime** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**instance_id**|**int**|Numero di identificazione univoco integer dell'istruzione.|  
|**source_server**|**nvarchar(30)**|Nome di computer del server da cui viene inviata l'istruzione. Nella [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] versione 7,0, questo è sempre il nome computer del server master (MSX).|  
|**operation_code**|**nvarchar(4000)**|Codice di operazione dell'istruzione.|  
|**object_name**|**sysname**|Oggetto su cui viene eseguita l'istruzione.|  
|**object_id**|**uniqueidentifier**|Numero di identificazione dell'oggetto interessato dall'istruzione (**job_id** per un oggetto processo o 0x00 per un oggetto server) o un valore di dati specifico per l' **operation_code**.|  
|**target_server**|**nvarchar(30)**|Server di destinazione in cui deve essere eseguito il download dell'istruzione.|  
|**error_message**|**nvarchar(1024)**|Eventuale messaggio di errore inviato dal server di destinazione se si verifica un problema durante l'elaborazione dell'istruzione.<br /><br /> Nota: tutti i messaggi di errore bloccano tutti gli altri download dal server di destinazione.|  
|**date_posted**|**datetime**|Data di inserimento dell'istruzione nella tabella.|  
|**date_downloaded**|**datetime**|Data di download dell'istruzione nel server di destinazione.|  
|**Stato**|**tinyint**|Stato del processo:<br /><br /> **0** = non ancora scaricato<br /><br /> **1** = il download è stato completato.|  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni di esecuzione per questa procedura vengono assegnate per impostazione predefinita ai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene visualizzato un elenco di righe in `sysdownloadlist` per il processo `NightlyBackups`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_downloadlist  
    @job_name = N'NightlyBackups',  
    @operation = N'UPDATE',   
    @object_type = N'JOB',   
    @object_name = N'NightlyBackups',  
    @target_server = N'SEATTLE2',   
    @has_error = 1,   
    @status = NULL,   
    @date_posted = NULL ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
