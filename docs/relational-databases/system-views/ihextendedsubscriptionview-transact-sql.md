---
description: IHextendedSubscriptionView (Transact-SQL)
title: Vista IHextendedSubscriptionView (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- IHextendedSubscriptionView_TSQL
- IHextendedSubscriptionView
dev_langs:
- TSQL
helpviewer_keywords:
- IHextendedSubscriptionView view
ms.assetid: 124756a4-463a-4a81-bf5b-de7e8ffc7a62
author: stevestein
ms.author: sstein
ms.openlocfilehash: 75498189c413aa89a81d2c0e8086c0f5cc37e1a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208886"
---
# <a name="ihextendedsubscriptionview-transact-sql"></a>IHextendedSubscriptionView (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La vista **vista IHextendedSubscriptionView** espone informazioni sulla sottoscrizione a una pubblicazione non SQL Server. Questa vista è archiviata nel database di **distribuzione** .  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**article_id**|**int**|ID univoco di un articolo.|  
|**dest_db**|**sysname**|Nome del database di destinazione.|  
|**srvid**|**smallint**|ID univoco di un Sottoscrittore.|  
|**login_name**|**sysname**|Account di accesso utilizzato per la connessione a un Sottoscrittore.|  
|**distribution_jobid**|**binary**|Identifica il processo dell'agente di distribuzione.|  
|**publisher_database_id**|**int**|Identifica il database di pubblicazione.|  
|**subscription_type**|**int**|Tipo di sottoscrizione:<br /><br /> **0** = push: l'agente di distribuzione viene eseguito nel Sottoscrittore.<br /><br /> **1** = pull: l'agente di distribuzione viene eseguito nel server di distribuzione.|  
|**sync_type**|**tinyint**|Tipo di sincronizzazione iniziale:<br /><br /> **1** = automatico<br /><br /> **2** = nessuna|  
|**Stato**|**tinyint**|Stato della sottoscrizione:<br /><br /> **0** = inattivo<br /><br /> **1** = sottoscritto<br /><br /> **2** = attivo|  
|**snapshot_seqno_flag**|**bit**|Indica se viene utilizzato un numero di sequenza dello snapshot.|  
|**independent_agent**|**bit**|Specifica se per la pubblicazione è disponibile un agente di distribuzione autonomo.<br /><br /> **0** = la pubblicazione utilizza un agente di distribuzione condiviso e ogni coppia database del server di pubblicazione/database del Sottoscrittore dispone di un solo agente condiviso.<br /><br /> **1** = è presente una agente di distribuzione autonoma per questa pubblicazione.|  
|**subscription_time**|**datetime**|Solo per uso interno.|  
|**loopback_detection**|**bit**|Si applica alle sottoscrizioni che fanno parte di una topologia di replica transazionale bidirezionale. Il rilevamento di loopback determina se l'agente di distribuzione deve inviare nuovamente al Sottoscrittore le transazioni provenienti dal Sottoscrittore:<br /><br /> **1** = non viene restituito.<br /><br /> **0** = restituisce.|  
|**agent_id**|**int**|ID univoco dell'agente di distribuzione.|  
|**update_mode**|**tinyint**|Indica il tipo di modalità di aggiornamento. I possibili valori sono i seguenti:<br /><br /> **0** = sola lettura.<br /><br /> **1** = aggiornamento immediato.<br /><br /> **2** = aggiornamento in coda tramite Accodamento messaggi.<br /><br /> **3** = aggiornamento immediato con aggiornamento in coda come failover tramite Accodamento messaggi.<br /><br /> **4** = aggiornamento in coda tramite SQL Server coda.<br /><br /> **5** = aggiornamento immediato con failover di aggiornamento in coda, usando SQL Server coda.|  
|**publisher_seqno**|**varbinary(16)**|Numero di sequenza della transazione nel server di pubblicazione per questa sottoscrizione.|  
|**ss_cplt_seqno**|**varbinary(16)**|Numero di sequenza utilizzato per indicare il completamento dell'elaborazione simultanea degli snapshot.|  
  
## <a name="see-also"></a>Vedere anche  
 [Replica di database eterogenei](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
