---
description: MSarticles (Transact-SQL)
title: MSarticles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSarticles
- MSarticles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSarticles system table
ms.assetid: 1acd79a5-b3e2-4161-9592-7acc2a41ba38
author: cawrites
ms.author: chadam
ms.openlocfilehash: c4f9badc2350511763ff41501887dc38a1a6c193
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094790"
---
# <a name="msarticles-transact-sql"></a>MSarticles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSarticles** contiene una riga per ogni articolo replicato da un server di pubblicazione. Questa tabella è archiviata nel database di distribuzione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**publisher_id**|**smallint**|ID del server di pubblicazione.|  
|**publisher_db**|**sysname**|Nome del database del server di pubblicazione.|  
|**publication_id**|**int**|ID della pubblicazione.|  
|**articolo**|**sysname**|Nome dell'articolo.|  
|**article_id**|**int**|ID dell'articolo.|  
|**destination_object**|**sysname**|Nome della tabella creata nel Sottoscrittore.|  
|**source_owner**|**sysname**|Nome dello schema della tabella di origine nel server di pubblicazione.|  
|**source_object**|**sysname**|Nome dell'oggetto di origine da cui aggiungere l'articolo.|  
|**description**|**nvarchar(255)**|Descrizione dell'articolo.|  
|**destination_owner**|**sysname**|Nome dello schema della tabella creata nel Sottoscrittore.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
