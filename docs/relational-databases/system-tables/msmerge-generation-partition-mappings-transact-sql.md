---
title: MSmerge_generation_partition_mappings (T-SQL)
description: Viene descritto il MSmerge_generation_partition_mappings stored procedure utilizzato per tenere traccia delle modifiche apportate alle partizioni in una pubblicazione di tipo merge.
ms.custom: seo-lt-2019
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_generation_partition_mappings_TSQL
- MSmerge_generation_partition_mappings
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_generation_partition_mappings system table
ms.assetid: 443a4024-ce48-4772-9ee5-95bd6fb6476b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7e4bc8d5e833f5ca4c9c19c81985f744c497dfcd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102060"
---
# <a name="msmerge_generation_partition_mappings-transact-sql"></a>MSmerge_generation_partition_mappings (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSmerge_generation_partition_mappings** viene utilizzata per tenere traccia delle modifiche apportate alle partizioni in una pubblicazione di tipo merge. Questa tabella viene archiviata nei database di pubblicazione e di sottoscrizione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**publication_number**|**smallint**|Identifica la pubblicazione di tipo merge.|  
|**generazione**|**bigint**|Valore di generazione.|  
|**partition_id**|**int**|Identifica la partizione.|  
|**changecount**|**int**|Numero di volte che la partizione è stata modificata.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
