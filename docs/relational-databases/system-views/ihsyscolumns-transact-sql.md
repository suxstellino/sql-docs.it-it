---
description: IHsyscolumns (Transact-SQL)
title: IHsyscolumns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- IHsyscolumns
- IHsyscolumns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IHsyscolumns view
ms.assetid: 263452f1-9708-48f0-9536-402a89e7f5bf
author: stevestein
ms.author: sstein
ms.openlocfilehash: c9c0733638fd79c025b94b2e63cf9b5350dd0178
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204308"
---
# <a name="ihsyscolumns-transact-sql"></a>IHsyscolumns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La vista **IHsyscolumns** espone le informazioni sulle colonne per gli articoli pubblicati da un server di pubblicazione non SQL Server. Questa vista è archiviata in DistributionDatabase.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**sysname**|Nome della colonna o del parametro della procedura.|  
|**id**|**int**|ID di oggetto della tabella a cui appartiene la colonna o ID della stored procedure a cui è associato il parametro.|  
|**xtype**|**tinyint**|Tipo di archiviazione fisica dei [ tipi disys.sys&#40;&#41;Transact-SQL ](../../relational-databases/system-compatibility-views/sys-systypes-transact-sql.md).|  
|**typestat**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**xusertype**|**tinyint**|ID del tipo di dati esteso definito dall'utente.|  
|**length**|**bigint**|Lunghezza massima di archiviazione fisica dei [ tipi disys.sys&#40;&#41;Transact-SQL ](../../relational-databases/system-compatibility-views/sys-systypes-transact-sql.md).|  
|**xprec**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**xscale**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**colid**|**int**|ID di colonna o di parametro.|  
|**xoffset**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**bitpos**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**riservati**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**colstat**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**cdefault**|**int**|ID del valore predefinito della colonna.|  
|**dominio**|**int**|ID della regola o vincolo CHECK per la colonna.|  
|**number**|**int**|Numero della sottoprocedura quando la procedura è raggruppata (**0** per le voci non di procedura).|  
|**colorder**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**autoval**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**offset**|**int**|Offset nella riga in cui appare la colonna.|  
|**CollationID**|**int**|ID delle regole di confronto della colonna. NULL per colonne non di tipo carattere.|  
|**language**|**int**|Identificatore di lingua per la colonna.|  
|**Stato**|**int**|Mappa di bit utilizzata per descrivere una proprietà della colonna o del parametro:<br /><br /> **0x08** = la colonna consente valori null.<br /><br /> **0x10** = il riempimento ANSI era attivo quando sono state aggiunte colonne **varchar** o **varbinary** . Gli spazi vuoti finali vengono conservati per **varchar** e gli zeri finali vengono conservati per le colonne **varbinary** .<br /><br /> **0x40** = il parametro è un parametro di output.<br /><br /> **0x80** = la colonna è una colonna Identity.|  
|**type**|**int**|Tipo di archiviazione fisica dei [ tipi disys.sys&#40;&#41;Transact-SQL ](../../relational-databases/system-compatibility-views/sys-systypes-transact-sql.md).|  
|**UserType**|**tinyint**|ID del tipo di dati definito dall'utente da [ tipi disys.sys&#40;&#41;Transact-SQL ](../../relational-databases/system-compatibility-views/sys-systypes-transact-sql.md).|  
|**printfmt**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**prec**|**int**|Livello di precisione della colonna|  
|**scale**|**int**|Scala della colonna.|  
|**IsComputed**|**int**|Flag che indica se si tratta di una colonna calcolata:<br /><br /> **0** = non calcolata.<br /><br /> **1** = calcolato.|  
|**isoutparam**|**int**|Indica se il parametro della procedura è un parametro di output:<br /><br /> **1** = true.<br /><br /> **0** = false.|  
|**IsNullable**|**int**|Indica se la colonna ammette valori Null:<br /><br /> **1** = true.<br /><br /> **0** = false.|  
|**confronto**|**int**|Nome delle regole di confronto della colonna. NULL per colonne non di tipo carattere.|  
|**tdscollation**|**int**|Nome delle regole di confronto della colonna quando restituite in un flusso di dati tabulare (TDS, Tabular Data Stream).|  
  
## <a name="see-also"></a>Vedere anche  
 [Replica di database eterogenei](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)  
  
  
