---
description: MSdbms_map (Transact-SQL)
title: MSdbms_map (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSdbms_map
- MSdbms_map_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSdbms_map system table
ms.assetid: df67e691-3a50-450a-99c5-8c4a041749ae
author: cawrites
ms.author: chadam
ms.openlocfilehash: ee51f669562f902addcba79d3186824132df342b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099631"
---
# <a name="msdbms_map-transact-sql"></a>MSdbms_map (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSdbms_map** contiene informazioni sul tipo di dati di origine, nonché un collegamento a informazioni sul tipo di dati di destinazione predefinito per le coppie DBMS di origine e di destinazione. Questa tabella è archiviata nel database **msdb** e viene utilizzata per la pubblicazione eterogenea.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**map_id**|**int**|Identifica in modo univoco un mapping di tipi di dati.|  
|**src_dbms_id**|**int**|Identifica il sistema DBMS di origine specificandone il **dbms_id** nella tabella [MSdbms](../../relational-databases/system-tables/msdbms-transact-sql.md) .|  
|**dest_dbms_id**|**int**|Identifica il sistema DBMS di destinazione specificandone il **dbms_id** nella tabella [MSdbms](../../relational-databases/system-tables/msdbms-transact-sql.md) .|  
|**src_datatype_id**|**int**|Identifica il **datatype_id** dalla tabella [MSdbms_datatype](../../relational-databases/system-tables/msdbms-datatype-transact-sql.md) per il tipo di dati di origine.|  
|**src_len_min**|**bigint**|Lunghezza minima del tipo di dati nel DBMS di origine. Il valore NULL indica che la lunghezza non viene utilizzata.|  
|**src_len_max**|**bigint**|Lunghezza massima del tipo di dati nel DBMS di origine. Il valore NULL indica che la lunghezza non viene utilizzata.|  
|**src_prec_min**|**bigint**|Precisione minima del tipo di dati nel DBMS di origine. Il valore NULL indica che la precisione non viene utilizzata.|  
|**src_prec_max**|**bigint**|Precisione massima del tipo di dati nel DBMS di origine. Il valore NULL indica che la precisione non viene utilizzata.|  
|**src_scale_min**|**int**|Scala minima del tipo di dati nel DBMS di origine. Il valore NULL indica che la scala non viene utilizzata.|  
|**src_scale_max**|**int**|Scala massima del tipo di dati nel DBMS di origine. Il valore NULL indica che la scala non viene utilizzata.|  
|**src_nullable**|**bit**|Specifica se la colonna di destinazione nel mapping ammette valori NULL. Il valore NULL indica che questa definizione non è necessaria.|  
|**default_datatype_mapping_id**|**int**|Identifica il mapping del tipo di dati predefinito specificandone il **map_id** nella tabella [MSdbms_datatype_mapping](../../relational-databases/system-tables/msdbms-datatype-mapping-transact-sql.md).|  
  
## <a name="see-also"></a>Vedere anche  
 [Replica di database eterogenei](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Specificare i mapping dei tipi di dati per un server di pubblicazione Oracle](../../relational-databases/replication/publish/specify-data-type-mappings-for-an-oracle-publisher.md)   
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
