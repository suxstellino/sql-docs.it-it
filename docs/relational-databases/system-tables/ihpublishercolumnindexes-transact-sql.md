---
description: IHpublishercolumnindexes (Transact-SQL)
title: IHpublishercolumnindexes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- IHpublishercolumnindexes
- IHpublishercolumnindexes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IHpublishercolumnindexes system table
ms.assetid: 95b95a1d-b502-4838-825f-82a456487e25
author: cawrites
ms.author: chadam
ms.openlocfilehash: 13d7e4240b86950b21889f660d9e228eb09552e7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201666"
---
# <a name="ihpublishercolumnindexes-transact-sql"></a>IHpublishercolumnindexes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella di sistema **IHpublishercolumnindexes** esegue il mapping delle colonne di una pubblicazione non SQL Server nella tabella di sistema [IHpublishercolumns](../../relational-databases/system-tables/ihpublishercolumns-transact-sql.md) agli indici nella tabella di sistema [IHpublisherindexes](../../relational-databases/system-tables/ihpublisherindexes-transact-sql.md) . Questa tabella è archiviata nel database di distribuzione.  
  
## <a name="definition"></a>Definizione  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**publishercolumn_id**|**int**|Identifica la colonna da [IHpublishercolumns](../../relational-databases/system-tables/ihpublishercolumns-transact-sql.md) con un indice associato.|  
|**publisherindex_id**|**int**|Identifica un indice della tabella [IHpublisherindexes](../../relational-databases/system-tables/ihpublisherindexes-transact-sql.md) associata alla colonna.|  
|**indid**|**int**|Indica la posizione della colonna nella tabella pubblicata.|  
  
## <a name="see-also"></a>Vedere anche  
 [Replica di database eterogenei](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
