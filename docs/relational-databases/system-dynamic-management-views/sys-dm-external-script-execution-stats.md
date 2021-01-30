---
description: sys.dm_external_script_execution_stats
title: sys.dm_external_script_execution_stats | Microsoft Docs
ms.custom: ''
ms.date: 06/25/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: reference
f1_keywords:
- sys.dm_external_script_execution_stats
- sys.dm_external_script_execution_stats_TSQL
- dm_external_script_execution_stats
- dm_external_script_execution_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_external_script_execution_stats dynamic management view
ms.assetid: 2e99f026-ceb2-42a2-a549-c71d31ed0cf4
author: dphansen
ms.author: davidph
manager: cgronlun
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 2885f83185218d389d2a6fcf7dce6c3fbd097286
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201344"
---
# <a name="sysdm_external_script_execution_stats"></a>sys.dm_external_script_execution_stats
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Restituisce una riga per ogni tipo di richiesta di script esterni. Le richieste di script esterni vengono raggruppate per il linguaggio di script esterno supportato. Viene generata una riga per ogni funzione di script esterni registrata. Le funzioni di script esterni arbitrarie non vengono registrate a meno che non siano inviate da un processo padre, ad esempio `rxExec`.
  
> [!NOTE]  
> Questa vista a gestione dinamica (DMV) è disponibile solo se è stata installata e abilitata la funzionalità che supporta l'esecuzione di script esterni. Per altre informazioni, vedere [r Services in SQL Server 2016](../../machine-learning/r/sql-server-r-services.md), [Machine Learning Services (r, Python) in SQL Server 2017 e versioni successive](../../machine-learning/sql-server-machine-learning-services.md) e [Azure istanza gestita Machine Learning Services](/azure/azure-sql/managed-instance/machine-learning-services-overview).
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|Linguaggio|**nvarchar**|Nome del linguaggio di script esterni registrato. Ogni script esterno deve specificare il linguaggio della richiesta di script per usare l'utilità di avvio associata. |  
|counter_name|**nvarchar**|Nome di una funzione di script esterni registrata. Non ammette i valori Null.|  
|counter_value|**integer**|Numero totale di istanze chiamate dalla funzione di script esterni registrata nel server. Questo valore è cumulativo, parte dall'ora di installazione della funzionalità nell'istanza e non può essere reimpostato.|  

## <a name="permissions"></a>Autorizzazioni

 È richiesta l'autorizzazione VIEW SERVER STATE nel server.  
  
> [!NOTE]  
> Gli utenti che eseguono script esterni devono avere l'autorizzazione aggiuntiva EXECUTE ANY EXTERNAL SCRIPT, tuttavia, questa DMV può essere usata dagli amministratori senza tale autorizzazione.
  
## <a name="remarks"></a>Commenti

  Questa DMV viene fornita per la telemetria interna, per monitorare l'utilizzo complessivo della nuova funzionalità di esecuzione di script esterni disponibile in [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]. Il servizio di telemetria viene avviato insieme al LaunchPad e viene incrementato di un contatore basato su disco ogni volta che viene chiamata una funzione di script esterni registrata.

In generale, i contatori delle prestazioni sono validi solo se il processo che li ha generati è attivo. Quindi, una query su una DMV non può visualizzare i dati dettagliati per i servizi che non sono in esecuzione. Ad esempio, se un utilità di avvio esegue uno script esterno e li completa molto rapidamente, una DMV convenzionale potrebbe non visualizzare dati.

Di conseguenza, i contatori rilevati da questa DMV restano in esecuzione e lo stato di sys.dm_external_script_requests viene mantenuto usando le scritture su disco, anche se l'istanza è arrestata.

### <a name="counter-values"></a>Valori contatore

In SQL Server 2016 l'unica lingua esterna supportata è R e le richieste di script esterni vengono gestite da [!INCLUDE[rsql_productname_md](../../includes/rsql-productname-md.md)] . In SQL Server 2017 e versioni successive e in Azure SQL Istanza gestita, R e Python sono linguaggi esterni supportati e le richieste di script esterni vengono gestite da [!INCLUDE[rsql_productname_md](../../includes/rsql-productnamenew-md.md)] .

Per R, questa DMV tiene traccia del numero di chiamate R effettuate su un'istanza. Ad esempio, se `rxLinMod` viene chiamato ed eseguito in parallelo, il contatore viene incrementato di 1.

Per il linguaggio R, i valori del contatore visualizzati nel campo *counter_name* rappresentano i nomi delle funzioni ScaleR registrate. I valori del campo *counter_value* rappresentano il numero complessivo di istanze per la funzione ScaleR specifica. 

Per Python, questa DMV tiene traccia del numero di chiamate Python effettuate su un'istanza.

Il conteggio inizia quando la funzionalità viene installata e abilitata nell'istanza ed è cumulativo fino a quando il file che gestisce lo stato viene eliminato o sovrascritto da un amministratore. Di conseguenza, in genere non è possibile reimpostare i valori in *counter_value*. Per monitorare l'utilizzo in base alla sessione, ai tempi del calendario o ad altri intervalli, si consiglia di acquisire i conteggi in una tabella.

### <a name="registration-of-external-script-functions-in-r"></a>Registrazione di funzioni di script esterni in R

R supporta gli script arbitrari e la community R fornisce molte migliaia di pacchetti, ognuno con funzioni e metodi propri. Tuttavia, questa DMV monitora solo le funzioni scaler installate con SQL Server 2016 R Services.

La registrazione di queste funzioni viene eseguita quando è installata la funzionalità e le funzioni registrate non possono essere aggiunte o eliminate.

## <a name="examples"></a>Esempio  
  
### <a name="viewing-the-number-of-r-scripts-run-on-the-server"></a>Visualizzazione del numero di script R eseguiti nel server

 L'esempio seguente visualizza il numero complessivo di esecuzioni di script esterni per il linguaggio R.  
  
```sql
SELECT counter_name, counter_value
FROM sys.dm_external_script_execution_stats
WHERE language = 'R';
```  

### <a name="viewing-the-number-of-python-scripts-run-on-the-server"></a>Visualizzazione del numero di script Python eseguiti sul server

Nell'esempio seguente viene visualizzato il numero cumulativo di esecuzioni di script esterni per il linguaggio Python.  
  
```sql
SELECT counter_name, counter_value
FROM sys.dm_external_script_execution_stats
WHERE language = 'Python';
```  

## <a name="see-also"></a>Vedi anche

+ [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
+ [Funzioni e viste a gestione dinamica relative all'esecuzione &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)
+ [sys.dm_external_script_requests](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-requests.md)  
+ [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)  
