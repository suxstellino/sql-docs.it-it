---
description: sys.dm_pdw_lock_waits (Transact-SQL)
title: sys.dm_pdw_lock_waits (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 8ef966f8-d14e-40d3-9626-3508ada9b8fb
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 9704cdc31250760b3e8946c713e3fdc40504a7c7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99142424"
---
# <a name="sysdm_pdw_lock_waits-transact-sql"></a>sys.dm_pdw_lock_waits (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Include informazioni sulle richieste in attesa di blocchi.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|wait_id|**bigint**|Posizione della richiesta nell'elenco di attesa.|ordinale in base 0. Questa operazione non è univoca in tutte le voci di attesa.|  
|session_id|**nvarchar(32)**|ID della sessione in cui si è verificato lo stato di attesa.|Vedere session_id in [sys.dm_pdw_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql.md).|  
|tipo|**nvarchar(255)**|Tipo di attesa rappresentata da questa voce.|Valori possibili:<br /><br /> Condiviso<br /><br /> SharedUpdate<br /><br /> ExclusiveUpdate<br /><br /> Esclusivo|  
|object_type|**nvarchar(255)**|Tipo di oggetto interessato dall'attesa.|Valori possibili:<br /><br /> OBJECT<br /><br /> DATABASE<br /><br /> SYSTEM<br /><br /> SCHEMA<br /><br /> APPLICATION|  
|object_name|**nvarchar (386)**|Nome o GUID dell'oggetto specificato interessato dall'attesa.|Le tabelle e le viste vengono visualizzate con nomi in tre parti.<br /><br /> Gli indici e le statistiche vengono visualizzati con nomi in quattro parti.<br /><br /> Nomi, entità e database sono nomi di stringa.|  
|request_id|**nvarchar(32)**|ID della richiesta in cui si è verificato lo stato di attesa.|ID della richiesta.<br /><br /> Si tratta di un GUID per le richieste di caricamento.|  
|request_time|**datetime**|Ora in cui è stato richiesto il blocco o la risorsa.||  
|acquire_time|**datetime**|Ora in cui è stato acquisito il blocco o la risorsa.||  
|state|**nvarchar(50)**|Stato dello stato di attesa.|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
|priority|**int**|Priorità dell'elemento in attesa.|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
  
## <a name="see-also"></a>Vedere anche  
 [Analisi delle sinapsi di Azure e DMV Parallel data warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
