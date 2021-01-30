---
description: sp_changesubstatus (Transact-SQL)
title: sp_changesubstatus (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changesubstatus
- sp_changesubstatus_TSQL
helpviewer_keywords:
- sp_changesubstatus
ms.assetid: 9370e47a-d128-4f15-9224-1c3642770c39
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 89de8f8a4c5f1abb3a07551bcca6eac53e0b2e2a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158970"
---
# <a name="sp_changesubstatus-transact-sql"></a>sp_changesubstatus (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifica lo stato di un Sottoscrittore esistente. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_changesubstatus [ [ @publication = ] 'publication' ]  
    [ , [ @article = ] 'article' ]  
    [ , [ @subscriber = ] 'subscriber' ]  
        , [ @status = ] 'status'  
    [ , [ @previous_status = ] 'previous_status' ]  
    [ , [ @destination_db = ] 'destination_db' ]  
    [ , [ @frequency_type = ] frequency_type ]  
    [ , [ @frequency_interval = ] frequency_interval ]  
    [ , [ @frequency_relative_interval = ] frequency_relative_interval ]  
    [ , [ @frequency_recurrence_factor = ] frequency_recurrence_factor ]  
    [ , [ @frequency_subday = ] frequency_subday ]  
    [ , [ @frequency_subday_interval = ] frequency_subday_interval ]  
    [ , [ @active_start_time_of_day = ] active_start_time_of_day ]  
    [ , [ @active_end_time_of_day = ] active_end_time_of_day ]  
    [ , [ @active_start_date = ] active_start_date ]  
    [ , [ @active_end_date = ] active_end_date ]  
    [ , [ @optional_command_line = ] 'optional_command_line' ]  
    [ , [ @distribution_jobid = ] distribution_jobid ]  
    [ , [ @from_auto_sync = ] from_auto_sync ]  
    [ , [ @ignore_distributor = ] ignore_distributor ]  
    [ , [ @offloadagent= ] remote_agent_activation ]  
    [ , [ @offloadserver= ] 'remote_agent_server_name' ]  
    [ , [ @dts_package_name= ] 'dts_package_name' ]  
    [ , [ @dts_package_password= ] 'dts_package_password' ]  
    [ , [ @dts_package_location= ] dts_package_location ]  
    [ , [ @skipobjectactivation = ] skipobjectactivation  
  [ , [ @distribution_job_name= ] 'distribution_job_name' ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e il valore predefinito è **%** . Se la *pubblicazione* non è specificata, verranno interessate tutte le pubblicazioni.  
  
`[ @article = ] 'article'` Nome dell'articolo. Deve essere univoco all'interno della pubblicazione. *article* è di **tipo sysname** e il valore predefinito è **%** . Se l' *articolo* non è specificato, vengono interessati tutti gli articoli.  
  
`[ @subscriber = ] 'subscriber'` Nome del Sottoscrittore di cui modificare lo stato. *Subscriber* è di **tipo sysname** e il valore predefinito è **%** . Se il *Sottoscrittore* non è specificato, lo stato viene modificato per tutti i sottoscrittori dell'articolo specificato.  
  
`[ @status = ] 'status'` Stato della sottoscrizione nella tabella **syssubscriptions** . *status* è di **tipo sysname** e non prevede alcun valore predefinito. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**active**|Il Sottoscrittore è sincronizzato e in fase di ricezione dei dati.|  
|**inactive**|Alla voce relativa al Sottoscrittore non è associata alcuna sottoscrizione.|  
|**subscribed**|Il Sottoscrittore richiede dati, ma non è ancora sincronizzato.|  
  
`[ @previous_status = ] 'previous_status'` Stato precedente della sottoscrizione. *previous_status* è di **tipo sysname** e il valore predefinito è null. Questo parametro consente di modificare le sottoscrizioni che hanno attualmente tale stato, consentendo così le funzioni di gruppo in un set specifico di sottoscrizioni, ad esempio impostando tutte le sottoscrizioni attive su **sottoscritte**.  
  
`[ @destination_db = ] 'destination_db'` Nome del database di destinazione. *destination_db* è di **tipo sysname** e il valore predefinito è **%** .  
  
`[ @frequency_type = ] frequency_type` Frequenza con cui pianificare l'attività di distribuzione. *frequency_type* è di **tipo int** e il valore predefinito è null.  
  
`[ @frequency_interval = ] frequency_interval` Valore da applicare alla frequenza impostata da *frequency_type*. *frequency_interval* è di **tipo int** e il valore predefinito è null.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Data dell'attività di distribuzione. Questo parametro viene usato quando *frequency_type* è impostato su 32 (mensile relativo). *frequency_relative_interval* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|First (Primo)|  
|**2**|Second|  
|**4**|Terzo|  
|**8**|Quarto|  
|**16**|Last (Ultimo)|  
|NULL (predefinito)||  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Fattore di occorrenza utilizzato da *frequency_type*. *frequency_recurrence_factor* è di **tipo int** e il valore predefinito è null.  
  
`[ @frequency_subday = ] frequency_subday` Frequenza, in minuti, di ripianificazione durante il periodo definito. *frequency_subday* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|Una sola volta|  
|**2**|Second|  
|**4**|Minuto|  
|**8**|Ora|  
|NULL (predefinito)||  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Intervallo per *frequency_subday*. *frequency_subday_interval* è di **tipo int** e il valore predefinito è null.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Ora del giorno in cui l'attività di distribuzione viene pianificata per la prima volta, formattata come HHMMSS. *active_start_time_of_day* è di **tipo int** e il valore predefinito è null.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Ora del giorno in cui l'attività di distribuzione smette di essere pianificata, formattata come HHMMSS. *active_end_time_of_day* è di **tipo int** e il valore predefinito è null.  
  
`[ @active_start_date = ] active_start_date` Data della prima pianificazione dell'attività di distribuzione, nel formato AAAAMMGG. *active_start_date* è di **tipo int** e il valore predefinito è null.  
  
`[ @active_end_date = ] active_end_date` Data in cui viene arrestata la pianificazione dell'attività di distribuzione, nel formato AAAAMMGG. *active_end_date* è di **tipo int** e il valore predefinito è null.  
  
`[ @optional_command_line = ] 'optional_command_line'` Prompt dei comandi facoltativo. *optional_command_line* è di **tipo nvarchar (4000)** e il valore predefinito è null.  
  
`[ @distribution_jobid = ] distribution_jobid` ID del processo del agente di distribuzione nel server di distribuzione per la sottoscrizione quando lo stato della sottoscrizione viene modificato da inattivo ad attivo. Negli altri casi non è definito. Se una chiamata a questa stored procedure richiede l'utilizzo di più agenti di distribuzione, il risultato non è definito. *distribution_jobid* è di tipo **Binary (16)** e il valore predefinito è null.  
  
`[ @from_auto_sync = ] from_auto_sync` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @ignore_distributor = ] ignore_distributor` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @offloadagent = ] remote_agent_activation`
 > [!NOTE]  
>  L'attivazione remota dell'agente è deprecata e non è più supportata. Questo parametro è supportato solo per compatibilità con gli script di versioni precedenti. Se si imposta *remote_agent_activation* su un valore diverso da **0** , viene generato un errore.  
  
`[ @offloadserver = ] 'remote_agent_server_name'`
 > [!NOTE]  
>  L'attivazione remota dell'agente è deprecata e non è più supportata. Questo parametro è supportato solo per compatibilità con gli script di versioni precedenti. L'impostazione di *remote_agent_server_name* su un valore diverso da null genera un errore.  
  
`[ @dts_package_name = ] 'dts_package_name'` Specifica il nome del pacchetto Data Transformation Services (DTS). *dts_package_name* è di **tipo sysname** e il valore predefinito è null. Per un pacchetto denominato **DTSPub_Package** , ad esempio, specificare `@dts_package_name = N'DTSPub_Package'` .  
  
`[ @dts_package_password = ] 'dts_package_password'` Consente di specificare la password per il pacchetto. *dts_package_password* è di **tipo sysname** e il valore predefinito è null, che indica che la proprietà della password deve essere lasciata invariata.  
  
> [!NOTE]  
>  A ogni pacchetto DTS deve essere associata una password.  
  
`[ @dts_package_location = ] dts_package_location` Specifica la posizione del pacchetto. *dts_package_location* è di **tipo int** e il valore predefinito è **0**. Se è **0**, il percorso del pacchetto si trova nel database di distribuzione. Se è **1**, il percorso del pacchetto si trova nel Sottoscrittore. Il percorso del pacchetto può essere **Distributor** o **Subscriber**.  
  
`[ @skipobjectactivation = ] skipobjectactivation` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @distribution_job_name = ] 'distribution_job_name'` Nome del processo di distribuzione. *distribution_job_name* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @publisher = ] 'publisher'` Specifica un server di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione non. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  Impossibile utilizzare *Publisher* quando si modificano le proprietà degli articoli in un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] server di pubblicazione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_changesubstatus** viene utilizzata nella replica snapshot e nella replica transazionale.  
  
 **sp_changesubstatus** modifica lo stato del Sottoscrittore nella tabella **syssubscriptions** con lo stato modificato. Se necessario, aggiorna lo stato dell'articolo nella tabella **sysarticles** per indicare attivo o inattivo. Se necessario, il flag di replica viene impostato su on o off nella tabella **sysobjects** per la tabella replicata.  
  
## <a name="permissions"></a>Autorizzazioni  
 È possibile eseguire **sp_changesubstatus** solo i membri del ruolo predefinito del server **sysadmin** , **db_owner** ruolo predefinito del database o l'autore della sottoscrizione.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_addsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)   
 [sp_dropsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)   
 [sp_helpdistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md)   
 [sp_helpsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
