---
description: IHpublisherconstraints (Transact-SQL)
title: IHpublisherconstraints (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- IHpublisherconstraints
- IHpublisherconstraints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IHpublisherconstraints system table
ms.assetid: 537b1e1a-7228-4680-aa27-5ad7072ea01e
author: cawrites
ms.author: chadam
ms.openlocfilehash: d53c7bdee115715b8513356f02a3afddc75ec118
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092453"
---
# <a name="ihpublisherconstraints-transact-sql"></a>IHpublisherconstraints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella di sistema **IHpublisherconstraints** contiene una riga per ogni vincolo replicato da server di pubblicazione non SQL Server che utilizzano il server di distribuzione corrente. Questa tabella è archiviata nel database di distribuzione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**publisherconstraint_id**|**int**|Identificatore del vincolo pubblicato.|  
|**table_id**|**int**|Identifica la tabella da [IHpublishertables](../../relational-databases/system-tables/ihpublishertables-transact-sql.md) a cui appartiene il vincolo.|  
|**publisher_id**|**smallint**|Identifica il server di pubblicazione non SQL Server da cui viene pubblicata la colonna.|  
|**Nome**|**Sysname**|Nome del vincolo pubblicato.|  
|**Tipo**|**nvarchar(255)**|Tipo di vincolo supportato dalla tabella di sistema [IHconstrainttypes](../../relational-databases/system-tables/ihconstrainttypes-transact-sql.md) .|  
  
## <a name="see-also"></a>Vedere anche  
 [Replica di database eterogenei](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
