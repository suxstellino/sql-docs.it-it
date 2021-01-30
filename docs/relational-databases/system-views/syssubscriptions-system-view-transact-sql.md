---
description: syssubscriptions (vista di sistema) (Transact-SQL)
title: syssubscriptions (vista di sistema) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- syssubscriptions_TSQL
- syssubscriptions
dev_langs:
- TSQL
helpviewer_keywords:
- syssubscriptions view
ms.assetid: c9613858-9512-43a9-aa53-7ee8064f064c
author: stevestein
ms.author: sstein
ms.openlocfilehash: 052d7d4c3fa302fbb5962f89f69f1446bd6e7295
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181543"
---
# <a name="syssubscriptions-system-view-transact-sql"></a>syssubscriptions (vista di sistema) (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La vista **syssubscriptions** espone informazioni sulla sottoscrizione. Questa vista è archiviata nel database di distribuzione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|ID univoco di un articolo sottoscritto.|  
|**srvid**|**smallint**|ID del Sottoscrittore.|  
|**dest_db**|**sysname**|Nome del database di sottoscrizione.|  
|**Stato**|**tinyint**|Stato della sottoscrizione:<br /><br /> **0** = inattivo.<br /><br /> **1** = sottoscritto.<br /><br /> **2** = attivo.|  
|**sync_type**|**tinyint**|Tipo di sincronizzazione iniziale:<br /><br /> **1** = automatico.<br /><br /> **2** = nessuna.|  
|**login_name**|**sysname**|Nome dell'account di accesso utilizzato per la connessione al server di pubblicazione per l'aggiunta della sottoscrizione.|  
|**subscription_type**|**int**|Tipo di sottoscrizione:<br /><br /> **0** = push: l'agente di distribuzione viene eseguito nel server di distribuzione.<br /><br /> **1** = pull: l'agente di distribuzione viene eseguito nel Sottoscrittore.|  
|**distribution_jobid**|**binary(16)**|Identifica il processo dell'agente di distribuzione utilizzato per sincronizzare la sottoscrizione.|  
|**timestamp**|**timestamp**|Data e ora di creazione della sottoscrizione.|  
|**update_mode**|**tinyint**|Modalità di aggiornamento:<br /><br /> **0** = sola lettura.<br /><br /> **1** = aggiornamento immediato.|  
|**loopback_detection**|**bit**|Si applica alle sottoscrizioni che fanno parte di una topologia di replica transazionale bidirezionale. Il rilevamento di loopback determina se l'agente di distribuzione deve inviare nuovamente al Sottoscrittore le transazioni provenienti dal Sottoscrittore:<br /><br /> **0** = restituisce.<br /><br /> **1** = non viene restituito.|  
|**queued_reinit**|**bit**|Specifica se l'articolo è contrassegnato per l'inizializzazione o la reinizializzazione. Il valore **1** indica che l'articolo sottoscritto è contrassegnato per l'inizializzazione o la reinizializzazione.|  
|**nosync_type**|**tinyint**|Tipo di inizializzazione della sottoscrizione:<br /><br /> **0** = automatico (snapshot)<br /><br /> **1** = solo supporto replica<br /><br /> **2** = inizializzazione con backup<br /><br /> **3** = inizializzazione dal numero di sequenza del file di log (LSN)<br /><br /> Per ulteriori informazioni, vedere il parametro **\@ sync_type** di [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md).<br /><br /> **3** = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**srvname**|**sysname**|Nome del Sottoscrittore.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [syssubscriptions &#40;Transact-SQL&#41;](../../relational-databases/system-tables/syssubscriptions-transact-sql.md)  
  
  
