---
title: sp_replmonitorhelppublicationthresholds (T-SQL)
description: Viene descritta la sp_replmonitorhelppublicationthresholds stored procedure che restituisce le metriche di soglia impostate per una pubblicazione monitorata.
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replmonitorhelppublicationthresholds
- sp_replmonitorhelppublicationthresholds_TSQL
helpviewer_keywords:
- sp_replmonitorhelppublicationthresholds
ms.assetid: d6b1aa4b-3369-4255-a892-c0e5cc9cb693
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1cfd6db6845702b624e98a31980a46f2fc6fd4f5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193918"
---
# <a name="sp_replmonitorhelppublicationthresholds-transact-sql"></a>sp_replmonitorhelppublicationthresholds (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Restituisce le metriche relative alle soglie impostate per una pubblicazione monitorata. Questa stored procedure, utilizzata per il monitoraggio della replica, viene eseguita nel database di distribuzione del server di distribuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_replmonitorhelppublicationthresholds [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'   
    [ , [ @publication_type = ] publication_type ]   
    [ , [ @thresholdmetricname = ] 'thresholdmetricname'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database pubblicato. *publisher_db* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publication_type = ] publication_type` Se il tipo di pubblicazione. *publication_type* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0**|Pubblicazione transazionale.|  
|**1**|Pubblicazione snapshot.|  
|**2**|Pubblicazione di tipo merge.|  
|NULL (predefinito)|La replica tenta di determinare il tipo di pubblicazione.|  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**metric_id**|**int**|ID della misurazione delle prestazioni di replica. I possibili valori sono i seguenti:<br /><br /> **1expiration** : monitora la scadenza imminente delle sottoscrizioni di pubblicazioni transazionali.<br /><br /> **2latency** : esegue il monitoraggio delle prestazioni delle sottoscrizioni di pubblicazioni transazionali.<br /><br /> **4mergeexpiration** : monitora la scadenza imminente delle sottoscrizioni di pubblicazioni di tipo merge.<br /><br /> **5mergeslowrunduration** : esegue il monitoraggio della durata delle sincronizzazioni di tipo merge su connessioni remote a larghezza di banda ridotta.<br /><br /> **6mergefastrunduration** : esegue il monitoraggio della durata delle sincronizzazioni di tipo merge su connessioni LAN a larghezza di banda elevata.<br /><br /> **7mergefastrunspeed** : esegue il monitoraggio della velocità di sincronizzazione delle sincronizzazioni di tipo merge su connessioni LAN a larghezza di banda elevata.<br /><br /> **8mergeslowrunspeed** : esegue il monitoraggio della velocità di sincronizzazione delle sincronizzazioni di tipo merge su connessioni remote a larghezza di banda ridotta.|  
|**title**|**sysname**|Nome della misurazione delle prestazioni di replica.|  
|**value**|**int**|Valore soglia della misurazione delle prestazioni.|  
|**shouldalert**|**bit**|Indica se deve essere generato un avviso quando la metrica supera la soglia definita per la pubblicazione. il valore **1** indica che deve essere generato un avviso.|  
|**IsEnabled**|**bit**|Indica se il monitoraggio è abilitato per questa misurazione delle prestazioni di replica per questa pubblicazione. il valore **1** indica che il monitoraggio è abilitato.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_replmonitorhelppublicationthresholds** viene utilizzato con tutti i tipi di replica.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del database **db_owner** o **replmonitor** nel database di distribuzione possono eseguire **sp_replmonitorhelppublicationthresholds**.  
  
## <a name="see-also"></a>Vedere anche  
 [Monitorare la replica a livello di programmazione](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
