---
description: sys.sp_cdc_change_job (Transact-SQL)
title: sys.sp_cdc_change_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sp_cdc_change_job_TSQL
- sys.sp_cdc_change_job
- sp_cdc_change_job_TSQL
- sp_cdc_change_job
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cdc_change_job
ms.assetid: ea918888-0fc5-4cc1-b301-26b2a9fbb20d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0dcc3a8b69e6cf8e65f96b239dd7699f796246ba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210754"
---
# <a name="syssp_cdc_change_job-transact-sql"></a>sys.sp_cdc_change_job (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica la configurazione di un processo di pulizia dell'acquisizione dei dati delle modifiche o di un processo di acquisizione nel database corrente. Per visualizzare la configurazione corrente di un processo, eseguire una query sulla tabella [dbo.cdc_jobs](../../relational-databases/system-tables/dbo-cdc-jobs-transact-sql.md) o utilizzare [sp_cdc_help_jobs](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-jobs-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.sp_cdc_change_job [ [ @job_type = ] 'job_type' ]  
    [ , [ @maxtrans = ] max_trans ]   
    [ , [ @maxscans = ] max_scans ]   
    [ , [ @continuous = ] continuous ]   
    [ , [ @pollinginterval = ] polling_interval ]   
    [ , [ @retention ] = retention ]   
    [ @threshold = ] 'delete threshold'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_type = ] 'job_type'` Tipo di processo da modificare. *job_type* è di **tipo nvarchar (20)** e il valore predefinito è' Capture '. Gli input validi sono 'capture' e 'cleanup'.  
  
`[ @maxtrans ] = max_trans_` Numero massimo di transazioni da elaborare in ogni ciclo di analisi. *max_trans* è di **tipo int** e il valore predefinito è null, che indica nessuna modifica per questo parametro. Se specificato, il valore deve essere un numero intero positivo.  
  
 *max_trans* è valido solo per i processi di acquisizione.  
  
`[ @maxscans ] = max_scans_` Numero massimo di cicli di analisi da eseguire per estrarre tutte le righe dal log. *max_scans* è di **tipo int** e il valore predefinito è null, che indica nessuna modifica per questo parametro.  
  
 *max_scan* è valido solo per i processi di acquisizione.  
  
`[ @continuous ] = continuous_` Indica se il processo di acquisizione deve essere eseguito in modo continuo (1) o solo una volta (0). *Continuous* è di **bit** e il valore predefinito è null, che indica nessuna modifica per questo parametro.  
  
 Quando *Continuous* = 1, il processo di [sp_cdc_scan](../../relational-databases/system-stored-procedures/sys-sp-cdc-scan-transact-sql.md) analizza il log ed elabora fino a (*max_trans* \* *max_scans*) transazioni. Attende quindi il numero di secondi specificato in *polling_interval* prima di iniziare la successiva analisi del log.  
  
 Quando *Continuous* = 0, il processo di **sp_cdc_scan** viene eseguito fino a *max_scans* le analisi del log, elaborando fino a *max_trans* transazioni durante ogni analisi e quindi esce.  
  
 Se **\@ Continuous** viene modificato da 1 a 0, **\@ pollingInterval** viene impostato automaticamente su 0. Un valore specificato per **\@ pollingInterval** diverso da 0 viene ignorato.  
  
 Se **\@ Continuous** viene omesso o impostato in modo esplicito su null e **\@ pollingInterval** viene impostato in modo esplicito su un valore maggiore di 0, **\@ Continuous** viene impostato automaticamente su 1.  
  
 *Continuous* è valido solo per i processi di acquisizione.  
  
`[ @pollinginterval ] = polling_interval_` Numero di secondi tra i cicli di analisi del log. *polling_interval* è di tipo **bigint** e il valore predefinito è null, che indica nessuna modifica per questo parametro.  
  
 *polling_interval* è valido solo per i processi di acquisizione quando *Continuous* è impostato su 1.  
  
`[ @retention ] = retention_` Numero di minuti per i quali le righe delle modifiche devono essere conservate nelle tabelle delle modifiche. la *conservazione* è di tipo **bigint** e il valore predefinito è null, che indica nessuna modifica per questo parametro. Il valore massimo è 52494800 (100 anni). Se specificato, il valore deve essere un numero intero positivo.  
  
 la *conservazione* è valida solo per i processi di pulizia.  
  
`[ @threshold = ] 'delete threshold'` Numero massimo di voci Delete che possono essere eliminate utilizzando una singola istruzione durante la pulizia. la *soglia di eliminazione* è di tipo **bigint** e il valore predefinito è null, che indica nessuna modifica per questo parametro. la *soglia di eliminazione* è valida solo per i processi di pulizia.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 Se un parametro viene omesso, il valore associato nella tabella [dbo.cdc_jobs](../../relational-databases/system-tables/dbo-cdc-jobs-transact-sql.md) non viene aggiornato. Un parametro impostato esplicitamente su NULL viene considerato come se fosse stato omesso.  
  
 Se si specifica un parametro non valido per il tipo di processo, l'istruzione avrà esito negativo.  
  
 Le modifiche apportate a un processo non diventano effettive fino a quando il processo non viene interrotto utilizzando [sp_cdc_stop_job](../../relational-databases/system-stored-procedures/sys-sp-cdc-stop-job-transact-sql.md) e riavviato utilizzando [sp_cdc_start_job](../../relational-databases/system-stored-procedures/sys-sp-cdc-start-job-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del database **db_owner** .  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-changing-a-capture-job"></a>R. Modifica di un processo di acquisizione  
 Nell'esempio seguente vengono aggiornati `@job_type` i `@maxscans` parametri, e `@maxtrans` di un processo di acquisizione nel `AdventureWorks2012` database. Gli altri parametri validi per un processo di acquisizione, `@continuous` e `@pollinginterval`, sono omessi e i relativi valori non vengono modificati.  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_change_job   
    @job_type = N'capture',  
    @maxscans = 1000,  
    @maxtrans = 15;  
GO  
```  
  
### <a name="b-changing-a-cleanup-job"></a>B. Modifica di un processo di pulizia  
 Nell'esempio seguente viene aggiornato un processo di pulizia nel database `AdventureWorks2012`. Vengono specificati tutti i parametri validi per questo tipo di processo, ad eccezione della **\@ soglia**. Il valore della **\@ soglia** non viene modificato.  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_change_job   
    @job_type = N'cleanup',  
    @retention = 2880;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [dbo.cdc_jobs &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/dbo-cdc-jobs-transact-sql.md)   
 [sys.sp_cdc_enable_table &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md)   
 [sys.sp_cdc_add_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md)  
  
  
