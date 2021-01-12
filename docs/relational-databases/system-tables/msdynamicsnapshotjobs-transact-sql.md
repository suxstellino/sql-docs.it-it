---
description: MSdynamicsnapshotjobs (Transact-SQL)
title: MSdynamicsnapshotjobs (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSdynamicsnapshotjobs_TSQL
- MSdynamicsnapshotjobs
dev_langs:
- TSQL
helpviewer_keywords:
- MSdynamicsnapshotjobs system table
ms.assetid: 4f36a325-0e3c-46c4-aeeb-416346cce0bc
author: cawrites
ms.author: chadam
ms.openlocfilehash: aadba278ff96ed4b9102ad748f86865e6072c2f0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094784"
---
# <a name="msdynamicsnapshotjobs-transact-sql"></a>MSdynamicsnapshotjobs (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSdynamicsnapshotjobs** tiene traccia delle informazioni sul filtro di riga con parametri applicate per generare uno snapshot dei dati filtrati. Questa tabella è archiviata nei database di pubblicazione e di sottoscrizione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID per il processo snapshot dei dati filtrati.|  
|**nome**|**sysname**|Nome del processo snapshot dei dati filtrati.|  
|**pubid**|**uniqueidentifier**|Numero di identificazione univoco della pubblicazione.|  
|**job_id**|**uniqueidentifier**|ID del processo di SQL Server Agent nel server di distribuzione.|  
|**agent_id**|**int**|ID di SQL Server Agent.|  
|**dynamic_filter_login**|**sysname**|Valore utilizzato per la valutazione della funzione [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) nei filtri di riga con parametri definiti per la pubblicazione.|  
|**dynamic_filter_hostname**|**sysname**|Valore utilizzato per la valutazione della funzione [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) nei filtri di riga con parametri definiti per la pubblicazione.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|Percorso della cartella in cui verranno letti i file di snapshot in caso di utilizzo di uno snapshot di dati filtrati.|  
|**partition_id**|**int**|ID della partizione di dati a cui appartiene il processo.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
