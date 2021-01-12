---
description: IHcolumns (Transact-SQL)
title: IHcolumns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- IHcolumns_TSQL
- IHcolumns
dev_langs:
- TSQL
helpviewer_keywords:
- IHcolumns system table
ms.assetid: 5bb027e5-5279-487b-9c33-5f402987253c
author: cawrites
ms.author: chadam
ms.openlocfilehash: e9d264dd2c3199527a763f5c2c8afce01dad17e3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094828"
---
# <a name="ihcolumns-transact-sql"></a>IHcolumns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella di sistema **IHcolumns** contiene una riga per ogni colonna pubblicata. Questa tabella viene utilizzata per definire la modalità di rappresentazione dei tipi di dati delle colonne dal server di pubblicazione non SQL Server quando vengono pubblicati, ovvero viene essenzialmente eseguito il mapping dei tipi di dati tra i sistemi di gestione DBMS (Database Management System, sistema di gestione di database) non SQL Server e SQL Server. Questa tabella è archiviata nel database di distribuzione.  
  
## <a name="definition"></a>Definizione  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**column_id**|**int**|Identifica una colonna pubblicata.|  
|**publishercolumn_id**|**int**|Associa una colonna pubblicata con i metadati della colonna archiviati nella tabella di sistema [IHpublishercolumns](../../relational-databases/system-tables/ihpublishercolumns-transact-sql.md) .|  
|**nome**|**sysname**|Specifica il nome della colonna.|  
|**article_id**|**int**|Identifica l'articolo al quale appartiene la colonna.|  
|**column_ordinal**|**int**|Identifica la colonna in base all'ordine.|  
|**mapped_type**|**tinyint**|Tipo di dati della colonna di destinazione nel Sottoscrittore.|  
|**mapped_length**|**bigint**|Lunghezza della colonna nel Sottoscrittore.|  
|**mapped_prec**|**int**|Precisione della colonna nel Sottoscrittore.|  
|**mapped_scale**|**int**|Scala della colonna nel Sottoscrittore.|  
|**mapped_nullable**|**bit**|Indica se la colonna nel Sottoscrittore accetta valori NULL, dove **1** indica che i valori null sono accettati.|  
  
## <a name="see-also"></a>Vedere anche  
 [Replica di database eterogenei](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_articlecolumn &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)   
 [sysarticlecolumns &#40;vista di sistema&#41; &#40;&#41;Transact-SQL ](../../relational-databases/system-views/sysarticlecolumns-system-view-transact-sql.md)   
 [sysarticlecolumns &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/sysarticlecolumns-transact-sql.md)  
  
  
