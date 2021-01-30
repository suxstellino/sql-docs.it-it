---
description: sys.dm_external_script_requests
title: sys.dm_external_script_requests | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: reference
f1_keywords:
- sys.dm_external_script_requests
- sys.dm_external_script_requests_TSQL
- dm_external_script_requests
- dm_external_script_requests_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_external_script_requests dynamic management view
ms.assetid: e7e7c50f-b8b2-403c-b8c8-1955da5636c3
author: dphansen
ms.author: davidph
manager: cgronlun
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: ca4cfdd348d04ab0332653fa6851107ffa5f16d1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207121"
---
# <a name="sysdm_external_script_requests"></a>sys.dm_external_script_requests
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Restituisce una riga per ogni account di lavoro attivo che esegue uno script esterno.
  
> [!NOTE]
> Questa vista a gestione dinamica (DMV) è disponibile solo se è stata installata e abilitata la funzionalità che supporta l'esecuzione di script esterni. Per altre informazioni, vedere [Machine Learning Services (r, Python) in SQL Server 2017 e versioni successive](../../machine-learning/sql-server-machine-learning-services.md), [r Services in SQL Server 2016](../../machine-learning/r/sql-server-r-services.md)e [Machine Learning Services in Azure SQL istanza gestita](/azure/azure-sql/managed-instance/machine-learning-services-overview).  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|external_script_request_id|**Identificatore univoco**|ID del processo che ha inviato la richiesta di script esterni. Corrisponde all'ID processo ricevuto dall'istanza di SQL.|  
|Linguaggio|**nvarchar**|Parola chiave che rappresenta un linguaggio di scripting supportato. |  
|degree_of_parallelism|**int**|Numero che indica il numero di processi paralleli che sono stati creati. Questo valore potrebbe essere diverso dal numero di processi paralleli che sono stati richiesti.|  
|external_user_name|**nvarchar**|Account di lavoro di Windows con cui è stato eseguito lo script.|  
  
## <a name="permissions"></a>Autorizzazioni

 È richiesta l' `VIEW SERVER STATE` autorizzazione per il server.  
  
> [!NOTE]
> Gli utenti che eseguono script esterni devono disporre dell'autorizzazione aggiuntiva `EXECUTE ANY EXTERNAL SCRIPT` , tuttavia questa DMV può essere usata dagli amministratori senza questa autorizzazione. 
  
## <a name="remarks"></a>Commenti  

Questa vista può essere filtrata usando l'identificatore del linguaggio di scripting.

La vista restituisce anche l'account di lavoro con cui viene eseguito lo script. Per informazioni sugli account di lavoro usati dagli script esterni, vedere la sezione identità usate nell'elaborazione (SQLRUserGroup) in [Panoramica della sicurezza per il Framework di estendibilità in SQL Server Machine Learning Services](../../machine-learning/concepts/security.md#sqlrusergroup).

Il GUID restituito nel campo **external_script_request_id** rappresenta anche il nome file della directory protetta in cui vengono archiviati i file temporanei. Ogni account di lavoro, ad esempio MSSQLSERVER01, rappresenta un singolo account di accesso SQL o utente di Windows e può essere utilizzato per eseguire più richieste di script. Per impostazione predefinita, la pulizia di questi file temporanei viene eseguita dopo il completamento dello script richiesto.

Questa DMV monitora solo i processi attivi e non può segnalare gli script che sono già stati completati. Se è necessario tenere traccia della durata degli script, è consigliabile aggiungere le informazioni sulla misurazione del tempo nello script e acquisire tali informazioni come parte dell'esecuzione dello script.

## <a name="examples"></a>Esempio  
  
### <a name="viewing-the-currently-active-scripts-for-a-particular-process"></a>Visualizzazione degli script attualmente attivi per un determinato processo

 L'esempio seguente visualizza il numero di esecuzioni dello script esterno completate nell'istanza corrente.  
  
```sql
SELECT external_script_request_id
  , [language]
  , degree_of_parallelism
  , external_user_name
FROM sys.dm_external_script_requests;
```  

Risultati  

external_script_request_id  |Linguaggio  |degree_of_parallelism  |external_user_name  
---------|---------|---------|---------
183EE6FC-7399-4318-AA2E-7A6C68E435A8     |     R    |      1   |  MSSQLSERVER01

## <a name="see-also"></a>Vedi anche

+ [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
+ [Funzioni e viste a gestione dinamica relative all'esecuzione &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
+ [sys.dm_external_script_execution_stats](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-execution-stats.md)
+ [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)  
