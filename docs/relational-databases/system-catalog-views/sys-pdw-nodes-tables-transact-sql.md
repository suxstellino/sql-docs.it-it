---
description: sys.pdw_nodes_tables (Transact-SQL)
title: sys.pdw_nodes_tables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 473b5d14-171b-4a16-9195-acf36d3f786c
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 2dd5f56a6b394d25ad8d1f2aa7e661a2bf21155e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207806"
---
# <a name="syspdw_nodes_tables-transact-sql"></a>sys.pdw_nodes_tables (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contiene una riga per ogni oggetto Table a cui appartiene un'entità o a cui l'entità ha concesso un'autorizzazione.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|\<inherited columns>||Per un elenco di colonne ereditate da questa vista, vedere [sys. Objects](../system-catalog-views/sys-objects-transact-sql.md).||  
|lob_data_space_id|**int**||Sempre 0.|  
|filestream_data_space_id|**int**|ID spazio dati per un filegroup FILESTREAM o [!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|NULL|  
|max_column_id_used|**int**|ID di colonna massimo utilizzato dalla tabella.||  
|lock_on_bulk_load|**bit**|La tabella è bloccata durante il caricamento bulk.|TBD|  
|uses_ansi_nulls|**bit**|La tabella è stata creata con l'opzione di database SET ANSI_NULLS impostata su ON.|1|  
|is_replicated|**bit**|1 = la tabella viene pubblicata utilizzando la replica.|0 la replica non è supportata.|  
|has_replication_filter|**bit**|1 = La tabella è associata a un filtro di replica.|0|  
|is_merge_published|**bit**|1 = La tabella è stata pubblicata tramite una replica di tipo merge.|0 non supportato.|  
|is_sync_tran_subscribed|**bit**|1 = La tabella è stata sottoscritta tramite una sottoscrizione ad aggiornamento immediato.|0 non supportato.|  
|has_unchecked_assembly_data|**bit**|1 = La tabella contiene dati persistenti che dipendono da un assembly la cui definizione è stata modificata durante l'ultima esecuzione di ALTER ASSEMBLY. Verrà reimpostato su 0 dopo la successiva esecuzione di DBCC CHECKDB o DBCC CHECKTABLE con esito positivo.|0 Nessun supporto CLR.|  
|text_in_row_limit|**int**|0 = L'opzione Text in row non è impostata.|Sempre 0.|  
|large_value_types_out_of_row|**bit**|1 = I tipi per valori di grandi dimensioni vengono archiviati esternamente alla riga.|Sempre 0.|  
|is_tracked_by_cdc|**bit**|1 = la tabella è abilitata per Change Data Capture|Sempre 0; Nessun supporto CDC.|  
|lock_escalation|**tinyint**|Il valore dell'opzione LOCK_ESCALATION per la tabella: 2 = AUTO|Sempre 2.|  
|lock_escalation_desc|**nvarchar(60)**|Descrizione testuale dell'opzione lock_escalation.|Sempre ꞌ AUTO ꞌ.|  
|pdw_node_id|**int**|Identificatore univoco di un [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] nodo.|NOT NULL|  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo di Azure Synapse Analytics e Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  
