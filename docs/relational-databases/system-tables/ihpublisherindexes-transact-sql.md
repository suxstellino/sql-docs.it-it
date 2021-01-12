---
description: IHpublisherindexes (Transact-SQL)
title: IHpublisherindexes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- IHpublisherindexes
- IHpublisherindexes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IHpublisherindexes system table
ms.assetid: 6008ef89-eeb9-46dc-93a2-f7623298cf0f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 03915f08ecf8aeb222f45e86af3988c64cab575b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092452"
---
# <a name="ihpublisherindexes-transact-sql"></a>IHpublisherindexes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella di sistema **IHpublisherindexes** contiene una riga per ogni indice replicato da server di pubblicazione non SQL Server che utilizzano il server di distribuzione corrente. Questa tabella è archiviata nel database di distribuzione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**publisherindex_id**|**int**|Identificatore dell'indice pubblicato.|  
|**table_id**|**int**|Identifica la tabella da [IHpublishertables](../../relational-databases/system-tables/ihpublishertables-transact-sql.md) a cui appartiene l'indice.|  
|**publisher_id**|**smallint**|Identificatore del server di pubblicazione non SQL Server dal quale l'indice viene pubblicato.|  
|**nome**|**sysname**|Nome dell'indice pubblicato.|  
|**type**|**nvarchar(255)**|Tipo di indice supportato dalla tabella di sistema [IHindextypes](../../relational-databases/system-tables/ihindextypes-transact-sql.md) .|  
  
## <a name="see-also"></a>Vedere anche  
 [Replica di database eterogenei](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
