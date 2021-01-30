---
description: sysdatatypemappings (Transact-SQL)
title: sysdatatypemappings (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysdatatypemappings
- sysdatatypemappings_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysdatatypemappings view
ms.assetid: 5dfafb70-3e3d-4465-b293-1acff1f855b6
author: stevestein
ms.author: sstein
ms.openlocfilehash: 753a18b314ea51fa341fe8e6f8b0fdfd235bf95b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160324"
---
# <a name="sysdatatypemappings-transact-sql"></a>sysdatatypemappings (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La vista **sysdatatypemappings** viene utilizzata per visualizzare il mapping tra SQL Server tipi di dati e i tipi di dati di un sistema di gestione di database non SQL Server (DBMS). Questa vista è archiviata nel database **msdb** .  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**mapping_id**|**int**|ID del mapping dei tipi di dati.|  
|**source_dbms**|**sysname**|Indica il nome del sistema DBMS per il quale viene eseguito il mapping dei tipi di dati. I possibili valori sono i seguenti.<br /><br /> **MSSQLSERVER** = l'origine è un database SQL Server.<br /><br /> **Oracle** = l'origine è un database Oracle.|  
|**source_version**|**sysname**|Indica la versione del prodotto del sistema DBMS di origine.|  
|**source_type**|**sysname**|Indica il tipo di dati elencato nel sistema DBMS di origine.|  
|**source_length_min**|**bigint**|Lunghezza minima del tipo di dati nel DBMS di origine. Il valore NULL indica che la lunghezza non viene utilizzata.|  
|**source_length_max**|**bigint**|Lunghezza massima del tipo di dati nel DBMS di origine. Il valore NULL indica che la lunghezza non viene utilizzata.|  
|**source_precision_min**|**bigint**|Precisione minima del tipo di dati nel DBMS di origine. Il valore NULL indica che la precisione non viene utilizzata.|  
|**source_precision_max**|**bigint**|Precisione massima del tipo di dati nel DBMS di origine. Il valore NULL indica che la precisione non viene utilizzata.|  
|**source_scale_min**|**int**|Scala minima del tipo di dati nel DBMS di origine. Il valore NULL indica che la scala non viene utilizzata.|  
|**source_scale_max**|**int**|Scala massima del tipo di dati nel DBMS di origine. Il valore NULL indica che la scala non viene utilizzata.|  
|**source_nullable**|**bit**|Indica se il tipo di dati di destinazione supporta i valori Null.|  
|**source_createparams**|**int**|Solo per uso interno.|  
|**destination_dbms**|**sysname**|Indica il nome del sistema DBMS di destinazione. I possibili valori sono i seguenti.<br /><br /> **MSSQLSERVER** = la destinazione è un database SQL Server.<br /><br /> **Oracle** = la destinazione è un database Oracle.<br /><br /> **DB2** = la destinazione è un database IBM DB2.<br /><br /> **Sybase** = la destinazione è un database Sybase.|  
|**destination_version**|**sysname**|Versione del prodotto del sistema DBMS di destinazione.|  
|**destination_type**|**sysname**|Tipo di dati nel sistema DBMS di destinazione.|  
|**destination_length**|**bigint**|Lunghezza del tipo di dati nel sistema DBMS di destinazione.|  
|**destination_precision**|**bigint**|Precisione del tipo di dati nel sistema DBMS di destinazione.|  
|**destination_scale**|**int**|Scala del tipo di dati nel sistema DBMS di destinazione.|  
|**destination_nullable**|**bit**|Indica se il tipo di dati nel sistema DBMS di destinazione supporta valori Null.|  
|**destination_createparams**|**int**|Solo per uso interno.|  
|**perdita**|**bit**|Indica se si verifica perdita dei dati durante l'esecuzione del mapping del tipo di dati tra il sistema DBMS di origine e quello di destinazione.|  
|**is_default**|**bit**|Indica se il mapping dei tipi di dati viene utilizzato per impostazione predefinita.|  
  
## <a name="see-also"></a>Vedere anche  
 [Replica di database eterogenei](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_helpdatatypemap &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpdatatypemap-transact-sql.md)  
  
  
