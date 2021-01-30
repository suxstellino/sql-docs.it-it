---
description: sp_replmonitorhelppublisher (Transact-SQL)
title: sp_replmonitorhelppublisher (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replmonitorhelppublisher_TSQL
- sp_replmonitorhelppublisher
helpviewer_keywords:
- sp_replmonitorhelppublisher
ms.assetid: 171501fe-4b74-4647-96c3-7691c777e01b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6252047b509af4311045185197994426c1d019ff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198287"
---
# <a name="sp_replmonitorhelppublisher-transact-sql"></a>sp_replmonitorhelppublisher (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Restituisce informazioni sullo stato corrente per uno o più server di pubblicazione associati a un server di distribuzione. Questa stored procedure, utilizzata per il monitoraggio della replica, viene eseguita nel database di distribuzione del server di distribuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_replmonitorhelppublisher [ [ @publisher = ] 'publisher' ]  
    [ , [ @refreshpolicy = ] refreshpolicy ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione di cui viene monitorato lo stato. *Publisher* è di **tipo sysname** e il valore predefinito è null. Se NULL, verranno restituite informazioni per tutti i server di pubblicazione che utilizzano il server di distribuzione.  
  
`[ @refreshpolicy = ] refreshpolicy` Solo per uso interno.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**pubblicazione**|**sysname**|Nome del server di pubblicazione.|  
|**distribution_db**|**sysname**|Nome del database di distribuzione utilizzato dal server di pubblicazione specificato.|  
|**Stato**|**int**|Stato massimo di tutti gli agenti di replica associati alle pubblicazioni nel server di pubblicazione specificato. I possibili valori sono i seguenti:<br /><br /> **1** = avviato<br /><br /> **2** = operazione completata<br /><br /> **3** = in corso<br /><br /> **4** = inattivo<br /><br /> **5** = nuovo tentativo<br /><br /> **6** = operazione non riuscita|  
|**warning**|**int**|Avviso correlato alla soglia massima generata da una sottoscrizione appartenente a una pubblicazione nel server di pubblicazione specificato. Può essere il risultato di un'operazione OR logica su uno o più dei valori seguenti.<br /><br /> **1** = scadenza: una sottoscrizione di una pubblicazione transazionale non è stata sincronizzata entro la soglia del periodo di memorizzazione.<br /><br /> **2** = latenza: il tempo impiegato per replicare i dati da un server di pubblicazione transazionale al Sottoscrittore supera la soglia, in secondi.<br /><br /> **4** = mergeexpiration-una sottoscrizione di una pubblicazione di tipo merge non è stata sincronizzata entro la soglia del periodo di memorizzazione.<br /><br /> **8** = mergefastrunduration-il tempo impiegato per completare la sincronizzazione di una sottoscrizione di tipo merge supera la soglia, in secondi, su una connessione di rete veloce.<br /><br /> **16** = mergeslowrunduration-il tempo impiegato per completare la sincronizzazione di una sottoscrizione di tipo merge supera la soglia, in secondi, su una connessione di rete lenta o remota.<br /><br /> **32** = mergefastrunspeed: la velocità di recapito delle righe durante la sincronizzazione di una sottoscrizione di tipo merge non è riuscita a mantenere la frequenza di soglia, in righe al secondo, su una connessione di rete veloce.<br /><br /> **64** = mergeslowrunspeed: la velocità di recapito delle righe durante la sincronizzazione di una sottoscrizione di tipo merge non è riuscita a mantenere la frequenza di soglia, in righe al secondo, su una connessione di rete lenta o remota.|  
|**publicationcount**|**int**|Numero di pubblicazioni appartenenti al server di pubblicazione.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_replmonitorhelppublisher** viene utilizzato con tutti i tipi di replica.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** nel server di distribuzione o i membri dei ruoli predefiniti del database **db_owner** o **replmonitor** nel database di distribuzione possono eseguire **sp_replmonitorhelppublisher**.  
  
## <a name="see-also"></a>Vedere anche  
 [Monitorare la replica a livello di programmazione](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
