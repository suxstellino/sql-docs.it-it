---
title: sp_replmonitorhelpmergesessiondetail (T-SQL)
description: Viene descritta la sp_replmonitorhelpmergesessiondetail stored procedure che restituisce informazioni dettagliate a livello di articolo su una specifica sessione di agente di merge di replica.
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replmonitorhelpmergesessiondetail
- sp_replmonitorhelpmergesessiondetail_TSQL
helpviewer_keywords:
- sp_replmonitorhelpmergesessiondetail
ms.assetid: 805c92fc-3169-410c-984d-f37e063b791d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ba050d49579e217fa5be5a2dedcd7aa21cf0c99d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204381"
---
# <a name="sp_replmonitorhelpmergesessiondetail-transact-sql"></a>sp_replmonitorhelpmergesessiondetail (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Restituisce informazioni dettagliate a livello di articolo su una specifica sessione di replica dell'agente di merge, utilizzato per monitorare la replica di tipo merge. Il set di risultati include una riga di dettaglio per ogni articolo sincronizzato durante la sessione, nonché una riga che rappresenta l'inizializzazione della sessione e righe che riepilogano le fasi di caricamento e scaricamento della sessione. Questa stored procedure viene eseguita nel database di distribuzione del server di distribuzione oppure nel database di sottoscrizione del Sottoscrittore.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_replmonitorhelpmergesessiondetail [ @session_id = ] session_id  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @session_id = ] session_id` Specifica una sessione di Agent. *session_id* è di **tipo int** e non prevede alcun valore predefinito.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**PhaseID**|**int**|Fase della sessione di sincronizzazione. I possibili valori sono i seguenti.<br /><br /> **0** = riga di inizializzazione o di riepilogo<br /><br /> **1** = caricamento<br /><br /> **2** = download|  
|**ArticleName**|**sysname**|Nome dell'articolo in fase di sincronizzazione. **ArticleName** contiene inoltre informazioni di riepilogo per le righe nel set di risultati che non rappresentano i dettagli degli articoli.|  
|**PercentComplete**|**decimal**|Indica la percentuale delle modifiche totali applicate in una riga di dettaglio dell'articolo per le sessioni in esecuzione e quelle non riuscite.|  
|**RelativeCost**|**decimal**|Indica il tempo dedicato alla sincronizzazione dell'articolo come percentuale del tempo totale di sincronizzazione per la sessione.|  
|**Duration**|**int**|Durata della sessione dell'agente.|  
|**Inserts**|**int**|Numero di inserimenti in una sessione.|  
|**Aggiornamenti**|**int**|Numero di aggiornamenti in una sessione.|  
|**Eliminazioni**|**int**|Numero di eliminazioni in una sessione.|  
|**Conflitti**|**int**|Numero di conflitti verificatisi in una sessione.|  
|**ErrorID**|**int**|ID di un errore di sessione.|  
|**SeqNo**|**int**|Ordine delle sessioni nel set di risultati.|  
|**RowType**|**int**|Indica il tipo di informazioni rappresentato da ogni riga nel set di risultati.<br /><br /> **0** = inizializzazione<br /><br /> **1** = Riepilogo del caricamento<br /><br /> **2** = dettagli di caricamento dell'articolo<br /><br /> **3** = Riepilogo download<br /><br /> **4** = dettagli sul download degli articoli|  
|**SchemaChanges**|**int**|Numero di modifiche dello schema in una sessione.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_replmonitorhelpmergesessiondetail** viene utilizzato per monitorare la replica di tipo merge.  
  
 Quando viene eseguito nel Sottoscrittore, **sp_replmonitorhelpmergesessiondetail** restituisce solo informazioni dettagliate sulle ultime 5 sessioni di agente di merge.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del database **db_owner** o **replmonitor** nel database di distribuzione nel server di distribuzione o nel database di sottoscrizione nel Sottoscrittore possono eseguire **sp_replmonitorhelpmergesessiondetail**.  
  
## <a name="see-also"></a>Vedere anche  
 [Monitorare la replica a livello di programmazione](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
