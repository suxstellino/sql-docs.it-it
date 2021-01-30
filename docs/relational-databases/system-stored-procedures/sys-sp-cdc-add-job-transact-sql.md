---
description: sys.sp_cdc_add_job (Transact-SQL)
title: sys.sp_cdc_add_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cdc_add_job_TSQL
- sys.sp_cdc_add_job
- sys.sp_cdc_add_job_TSQL
- sp_cdc_add_job
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cdc_add_job
ms.assetid: c4458738-ed25-40a6-8294-a26ca5a05bd9
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1c48413f1f588ea4a70c4f8a1190e819758c76b2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186697"
---
# <a name="syssp_cdc_add_job-transact-sql"></a>sys.sp_cdc_add_job (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Consente di creare un processo di pulizia dell'acquisizione dei dati delle modifiche o un processo di acquisizione nel database corrente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.sp_cdc_add_job [ @job_type = ] 'job_type'  
    [ , [ @start_job = ] start_job ]   
    [ , [ @maxtrans = ] max_trans ]   
    [ , [ @maxscans = ] max_scans ]   
    [ , [ @continuous = ] continuous ]   
    [ , [ @pollinginterval = ] polling_interval ]   
    [ , [ @retention ] = retention ]   
    [ , [ @threshold ] = 'delete_threshold' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_type = ] 'job\_type'` Tipo di processo da aggiungere. *job_type* è di **tipo nvarchar (20)** e non può essere null. Gli input validi sono **' Capture '** e **' Cleanup '**.  
  
`[ @start_job = ] start_job` Flag che indica se il processo deve essere avviato immediatamente dopo l'aggiunta. *START_JOB* è di **bit** e il valore predefinito è 1.  
  
`[ @maxtrans ] = max_trans` Numero massimo di transazioni da elaborare in ogni ciclo di analisi. *max_trans* è di **tipo int** e il valore predefinito è 500. Se specificato, il valore deve essere un numero intero positivo.  
  
 *max_trans* è valido solo per i processi di acquisizione.  
  
`[ @maxscans ] = max\_scans_` Numero massimo di cicli di analisi da eseguire per estrarre tutte le righe dal log. *max_scans* è di **tipo int** e il valore predefinito è 10.  
  
 *max_scan* è valido solo per i processi di acquisizione.  
  
`[ @continuous ] = continuous_` Indica se il processo di acquisizione deve essere eseguito in modo continuo (1) o solo una volta (0). *Continuous* è di **bit** e il valore predefinito è 1.  
  
 Quando *Continuous* = 1, il processo di [sp_cdc_scan](../../relational-databases/system-stored-procedures/sys-sp-cdc-scan-transact-sql.md) analizza il log ed elabora fino a (*max_trans* \* *max_scans*) transazioni. Attende quindi il numero di secondi specificato in *polling_interval* prima di iniziare la successiva analisi del log.  
  
 Quando *Continuous* = 0, il processo di **sp_cdc_scan** viene eseguito fino a *max_scans* le analisi del log, elaborando fino a *max_trans* transazione durante ogni analisi e quindi esce.  
  
 *Continuous* è valido solo per i processi di acquisizione.  
  
`[ @pollinginterval ] = polling\_interval_` Numero di secondi tra i cicli di analisi del log. *polling_interval* è di tipo **bigint** e il valore predefinito è 5.  
  
 *polling_interval* è valido solo per i processi di acquisizione quando *Continuous* è impostato su 1. Se specificato, il valore deve essere maggiore o uguale a 0 e inferiore a 24 ore (max: 86399 secondi). Se viene specificato il valore 0, non esiste alcun intervallo di attesa tra le analisi del log.  
  
`[ @retention ] = retention_` Numero di minuti per i quali le righe dei dati delle modifiche devono essere conservate nelle tabelle delle modifiche. la *conservazione* è di tipo **bigint** e il valore predefinito è 4320 (72 ore). Il valore massimo è 52494800 (100 anni). Se specificato, il valore deve essere un numero intero positivo.  
  
 la *conservazione* è valida solo per i processi di pulizia.  
  
`[ @threshold = ] 'delete\_threshold'` Numero massimo di voci Delete che possono essere eliminate utilizzando una singola istruzione durante la pulizia. *delete_threshold* è di tipo **bigint** e il valore predefinito è 5000.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 Un processo di pulizia viene creato utilizzando i valori predefiniti quando la prima tabella nel database è abilitata per Change Data Capture. Un processo di acquisizione viene creato utilizzando i valori predefiniti quando la prima tabella nel database è abilitata per Change Data Capture e per il database non esistono pubblicazioni transazionali. Se esiste una pubblicazione transazionale, per attivare il meccanismo di acquisizione viene utilizzato l'agente di lettura del log delle transazioni. Non è quindi necessario né consentito un processo di acquisizione separato.  
  
 Poiché i processi di pulizia e di acquisizione vengono creati per impostazione predefinita, questa stored procedure è necessaria solo quando un processo è stato eliminato in modo esplicito e deve essere ricreato.  
  
 Il nome del processo è **CDC.** _\<database\_name\>_ **\_ Cleanup** o **CDC.** _\<database\_name\>_ **\_ acquisizione**, dove *<database_name>* è il nome del database corrente. Se esiste già un processo con lo stesso nome, il nome viene aggiunto con un punto (**.**) seguito da un identificatore univoco, ad esempio: **CDC.AdventureWorks_capture. A1ACBDED-13FC-428C-8302-10100EF74F52**.  
  
 Per visualizzare la configurazione corrente di un processo di pulizia o di acquisizione, utilizzare [sp_cdc_help_jobs](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-jobs-transact-sql.md). Per modificare la configurazione di un processo, utilizzare [sp_cdc_change_job](../../relational-databases/system-stored-procedures/sys-sp-cdc-change-job-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del database **db_owner** .  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-capture-job"></a>R. Creazione di un processo di acquisizione  
 Nell'esempio seguente viene creato un processo di acquisizione. Questo esempio presuppone che il processo di pulizia esistente sia stato eliminato in modo esplicito e debba essere ricreato. Il processo viene creato utilizzando i valori predefiniti.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_cdc_add_job @job_type = N'capture';  
GO  
```  
  
### <a name="b-creating-a-cleanup-job"></a>B. Creazione di un processo di pulizia  
 Nell'esempio seguente viene creato un processo di pulizia nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Il parametro `@start_job` è impostato su 0 e `@retention` è impostato su 5760 minuti (96 ore). Questo esempio presuppone che il processo di pulizia esistente sia stato eliminato in modo esplicito e debba essere ricreato.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_cdc_add_job  
     @job_type = N'cleanup'  
    ,@start_job = 0  
    ,@retention = 5760;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [dbo.cdc_jobs &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/dbo-cdc-jobs-transact-sql.md)   
 [sys.sp_cdc_enable_table &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md)   
 [Informazioni su Change Data Capture &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
  
