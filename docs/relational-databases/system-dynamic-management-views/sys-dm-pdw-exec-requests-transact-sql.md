---
description: sys.dm_pdw_exec_requests (Transact-SQL)
title: sys.dm_pdw_exec_requests (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/05/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 390225cc-23e8-4051-a5f6-221e33e4c0b4
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 44dc108960c3fe8cda9e632802185f0472eb0373
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99142692"
---
# <a name="sysdm_pdw_exec_requests-transact-sql"></a>sys.dm_pdw_exec_requests (Transact-SQL)

[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Include informazioni su tutte le richieste attualmente o di recente attive in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] . Elenca una riga per ogni richiesta/query.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|request_id|**nvarchar(32)**|Chiave per questa visualizzazione. ID numerico univoco associato alla richiesta.|Univoco tra tutte le richieste nel sistema.|  
|session_id|**nvarchar(32)**|ID numerico univoco associato alla sessione in cui è stata eseguita la query. Vedere [sys.dm_pdw_exec_sessions &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql.md).||  
|status|**nvarchar(32)**|Stato corrente della richiesta.|' Running ',' suspended ',' completed ',' Canceled ',' failed '.|  
|submit_time|**datetime**|Ora in cui la richiesta è stata inviata per l'esecuzione.|**DateTime** valido minore o uguale all'ora corrente e start_time.|  
|start_time|**datetime**|Ora in cui è stata avviata l'esecuzione della richiesta.|NULL per le richieste in coda; in caso contrario, **DateTime** valido minore o uguale all'ora corrente.|  
|end_compile_time|**datetime**|Ora di completamento della compilazione della richiesta da parte del motore.|NULL per le richieste che non sono state ancora compilate; in caso contrario, un valore **DateTime** valido minore di start_time e minore o uguale all'ora corrente.|
|end_time|**datetime**|Ora in cui l'esecuzione della richiesta è stata completata, non riuscita o è stata annullata.|Null per le richieste in coda o attive; in caso contrario, un valore **DateTime** valido minore o uguale all'ora corrente.|  
|total_elapsed_time|**int**|Tempo trascorso nell'esecuzione dall'avvio della richiesta, in millisecondi.|Compreso tra 0 e la differenza tra submit_time e end_time.</br></br> Se total_elapsed_time supera il valore massimo per un numero intero, total_elapsed_time continuerà a essere il valore massimo. Questa condizione genererà l'avviso "è stato superato il valore massimo".</br></br> Il valore massimo in millisecondi è uguale a 24,8 giorni.|  
|label|**nvarchar(255)**|Stringa di etichetta facoltativa associata ad alcune istruzioni di query SELECT.|Qualsiasi stringa contenente ' a-z ',' A-Z ',' 0-9',' _'.|  
|error_id|**nvarchar (36)**|ID univoco dell'errore associato alla richiesta, se disponibile.|Vedere [sys.dm_pdw_errors &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-errors-transact-sql.md); impostare su NULL se non si è verificato alcun errore.|  
|database_id|**int**|Identificatore del database usato dal contesto esplicito, ad esempio usare DB_X.|Vedere ID in [sys. databases &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).|  
|.|**nvarchar(4000)**|Include il testo completo della richiesta come inviato dall'utente.|Qualsiasi testo di query o richiesta valido. Le query che hanno più di 4000 byte vengono troncate.|  
|resource_class|**nvarchar (20)**|Gruppo del carico di lavoro utilizzato per la richiesta. |Classi di risorse statiche</br>staticrc10</br>staticrc20</br>staticrc30</br>staticrc40</br>staticrc50</br>staticrc60</br>staticrc70</br>staticrc80</br>            </br>Classi di risorse dinamiche</br>SmallRC</br>MediumRC</br>LargeRC</br>XLargeRC|
|importance|**nvarchar(128)**|Importanza dell'impostazione della richiesta eseguita in.  Si tratta dell'importanza relativa di una richiesta nel gruppo del carico di lavoro e nei gruppi di carico di lavoro per le risorse condivise.  L'importanza specificata nel classificatore sostituisce l'impostazione di importanza del gruppo di carico di lavoro.</br>Si applica a: Azure Synapse Analytics|NULL</br>low</br>below_normal</br>normale (impostazione predefinita)</br>above_normal</br>high|
|group_name|**sysname** |Per le richieste che utilizzano risorse, group_name è il nome del gruppo di carico di lavoro in cui viene eseguita la richiesta.  Se la richiesta non utilizza risorse, group_name è null.</br>Si applica a: Azure Synapse Analytics|
|classifier_name|**sysname**|Per le richieste che usano risorse, il nome del classificatore usato per l'assegnazione delle risorse e l'importanza.||
|resource_allocation_percentage|**Decimal (5, 2)**|Percentuale di risorse allocate alla richiesta.</br>Si applica a: Azure Synapse Analytics|
|result_cache_hit|**int**|Indica in dettaglio se una query completata ha utilizzato la cache del set di risultati.  </br>Si applica a: Azure Synapse Analytics| 1 = riscontri nella cache del set di risultati </br> 0 = mancato riscontro nella cache del set di risultati </br> Valori interi negativi = motivi per cui la memorizzazione nella cache del set di risultati non è stata usata.  Per informazioni dettagliate, vedere la sezione Osservazioni.|
|client_correlation_id|**nvarchar(255)**|Nome facoltativo definito dall'utente per una sessione client.  Per impostare una sessione, chiamare sp_set_session_context ' client_correlation_id ',' <CorrelationIDName> '.  Eseguire `SELECT SESSION_CONTEXT(N'client_correlation_id')` per recuperare il valore.|
||||

## <a name="remarks"></a>Commenti 
 Per informazioni sul numero massimo di righe mantenute da questa visualizzazione, vedere la sezione metadati nell'argomento [limiti di capacità](/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#metadata) .

Il valore integer negativo nella colonna result_cache_hit è un valore bitmap di tutti i motivi applicati perché il set di risultati di una query non può essere memorizzato nella cache.  Questa colonna può essere la [| (OR bit per bit)](../../t-sql/language-elements/bitwise-or-transact-sql.md) prodotto di uno o più dei valori seguenti:  
  
|Valore            |Descrizione  |  
|-----------------|-----------------|  
|**1**|Riscontro nella cache del set di risultati|  
|**0x00** (**0**)|Mancata memorizzazione nella cache del set di risultati|  
|-**0x01** (**-1**)|La memorizzazione nella cache del set di risultati è disabilitata nel database.|  
|-**0x02** (**-2**)|La memorizzazione nella cache del set di risultati è disabilitata nella sessione. | 
|-**0x04** (**-4**)|La memorizzazione nella cache del set di risultati è disabilitata perché non sono presenti origini dati per la query.|  
|-**0x08** (**-8**)|Il caching del set di risultati è disabilitato a causa di predicati di sicurezza a livello di riga.|  
|-**0x10** (**-16**)|La memorizzazione nella cache del set di risultati è disabilitata a causa dell'utilizzo della tabella di sistema, della tabella temporanea o della tabella esterna della query.|  
|-**0x20** (**-32**)|La memorizzazione nella cache del set di risultati è disabilitata perché la query contiene costanti di runtime, funzioni definite dall'utente o funzioni non deterministiche.|  
|-**0x40**(**-64**)|La memorizzazione nella cache del set di risultati è disabilitata perché le dimensioni stimate del set di risultati sono >10 GB|  
|-**0x80**(**-128**) |La memorizzazione nella cache del set di risultati è disabilitata perché il set di risultati contiene righe di grandi dimensioni (>64KB).|  
|-**0x100**(**-256**) |La memorizzazione nella cache del set di risultati è disabilitata a causa dell'utilizzo della maschera dati dinamica granulare.|  

## <a name="permissions"></a>Autorizzazioni

 È richiesta l'autorizzazione VIEW SERVER STATE.  
  
## <a name="security"></a>Sicurezza

 sys.dm_pdw_exec_requests non filtra i risultati della query in base alle autorizzazioni specifiche del database. Gli account di accesso con autorizzazione VIEW SERVER STATE possono ottenere risultati di query per tutti i database  
  
>[!WARNING]  
>Un utente malintenzionato può utilizzare sys.dm_pdw_exec_requests per recuperare informazioni su oggetti di database specifici semplicemente disponendo dell'autorizzazione VIEW SERVER STATE e non dispone di autorizzazioni specifiche per il database.  
  
## <a name="see-also"></a>Vedere anche

 [Analisi delle sinapsi di Azure e DMV Parallel data warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)
