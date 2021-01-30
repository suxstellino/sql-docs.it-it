---
description: MSreplmonthresholdmetrics (Transact-SQL)
title: MSreplmonthresholdmetrics (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- msreplmonthresholdmetrics_TSQL
- msreplmonthresholdmetrics
dev_langs:
- TSQL
helpviewer_keywords:
- MSreplmonthresholdmetrics system table
ms.assetid: 0cc9b40a-36ce-485b-9bc2-d4abd5aa6727
author: cawrites
ms.author: chadam
ms.openlocfilehash: dfa858af359e203101e2fbe6a3205dea0134ddb2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193199"
---
# <a name="msreplmonthresholdmetrics-transact-sql"></a>MSreplmonthresholdmetrics (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSreplmonthresholdmetrics** definisce le metriche fornite per il monitoraggio della replica. Questa tabella è archiviata nel database **msdb** .  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**metric_id**|**int**|Identifica una misurazione delle prestazioni di replica. Ecco i possibili valori:<br /><br /> **1** = scadenza<br /><br /> **2** = latenza<br /><br /> **4** = mergeexpiration<br /><br /> **5** = mergeslowrunduration<br /><br /> **6** = mergefastrunduration<br /><br /> **7** = mergefastrunspeed<br /><br /> **8** = mergeslowrunspeed|  
|**title**|**sysname**|Nome della misurazione delle prestazioni di replica.|  
|**warningbitstatus**|**int**|Identificatore bit per bit utilizzato per generare un avviso per la violazione del valore soglia di una delle misurazioni seguenti:<br /><br /> **1** = scadenza: una sottoscrizione di una pubblicazione transazionale ha superato il periodo di memorizzazione superiore alla soglia consentita, come percentuale del periodo di memorizzazione.<br /><br /> **2** = latenza: il tempo impiegato per replicare i dati da un server di pubblicazione transazionale al Sottoscrittore supera la soglia, in secondi.<br /><br /> **4** = mergeexpiration-una sottoscrizione di una pubblicazione di tipo merge ha superato il periodo di memorizzazione superiore alla soglia consentita, come percentuale del periodo di memorizzazione.<br /><br /> **8** = mergefastrunduration-il tempo impiegato per completare la sincronizzazione di una sottoscrizione di tipo merge supera la soglia, in secondi, su una connessione di rete veloce.<br /><br /> **16** = mergeslowrunduration-il tempo impiegato per completare la sincronizzazione di una sottoscrizione di tipo merge supera la soglia, in secondi, su una connessione di rete lenta o remota.<br /><br /> **32** = mergefastrunspeed: la velocità di recapito delle righe durante la sincronizzazione di una sottoscrizione di tipo merge non è riuscita a mantenere la frequenza di soglia, in righe al secondo, su una connessione di rete veloce.<br /><br /> **64** = mergeslowrunspeed: la velocità di recapito delle righe durante la sincronizzazione di una sottoscrizione di tipo merge non è riuscita a mantenere la frequenza di soglia, in righe al secondo, su una connessione di rete lenta o remota.|  
|**alertmessageid**|**int**|ID del messaggio di errore visualizzato quando si verifica la condizione per la generazione di un avviso per la violazione del valore soglia.|  
|**description**|**nvarchar (3000)**|Descrizione della misurazione delle prestazioni di replica.|  
|**default_value**|**sql_variant**|Valore predefinito della misurazione delle prestazioni di replica.|  
|**min_value**|**sql_variant**|Valore minimo per una misurazione delle prestazioni di replica con limitazioni.|  
|**max_value**|**sql_variant**|Valore massimo per una misurazione delle prestazioni di replica con limitazioni.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
