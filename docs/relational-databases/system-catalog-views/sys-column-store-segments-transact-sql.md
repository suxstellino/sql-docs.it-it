---
description: sys.column_store_segments (Transact-SQL)
title: sys.column_store_segments (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/24/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- column_store_segments
- sys.column_store_segments_TSQL
- sys.column_store_segments
- column_store_segments_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.column_store_segments catalog view
ms.assetid: 1253448c-2ec9-4900-ae9f-461d6b51b2ea
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 51c5e31a97a52e94a8a6d164c6d43425451494b1
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2021
ms.locfileid: "102463969"
---
# <a name="syscolumn_store_segments-transact-sql"></a>sys.column_store_segments (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

Restituisce una riga per ogni segmento di colonna in un indice columnstore. È presente un segmento di colonna per colonna per ogni rowgroup. Ad esempio, una tabella con 10 colonne RowGroups e 34 restituisce 340 righe. 
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**partition_id**|**bigint**|Indica l'ID della partizione. Valore univoco all'interno di un database.|  
|**hobt_id**|**bigint**|ID dell'indice heap o albero B (HoBT) per la tabella con l'indice columnstore.|  
|**column_id**|**int**|ID della colonna columnstore.|  
|**segment_id**|**int**|ID di rowgroup. Per compatibilità con le versioni precedenti, il nome della colonna continua a essere chiamato segment_id anche se si tratta dell'ID rowgroup. È possibile identificare in modo univoco un segmento usando \<hobt_id, partition_id, column_id> , <segment_id>.|  
|**version**|**int**|Versione del formato del segmento di colonna.|  
|**encoding_type**|**int**|Tipo di codifica usato per il segmento:<br /><br /> 1 = VALUE_BASED-non stringa/binario senza dizionario (simile a 4 con alcune varianti interne)<br /><br /> 2 = colonna VALUE_HASH_BASED-non stringa/binaria con valori comuni nel dizionario<br /><br /> 3 = colonna STRING_HASH_BASED-String/Binary con valori comuni nel dizionario<br /><br /> 4 = STORE_BY_VALUE_BASED-non stringa/binario senza dizionario<br /><br /> 5 = STRING_STORE_BY_VALUE_BASED-String/Binary senza dizionario<br /><br /> Per ulteriori informazioni, vedere la sezione [osservazioni](#remarks) .|  
|**row_count**|**int**|Numero di righe nel gruppo di righe.|  
|**has_nulls**|**int**|1 se il segmento di colonna contiene valori Null.|  
|**base_id**|**bigint**|ID valore di base se viene utilizzato il tipo di codifica 1. Se non viene utilizzato il tipo di codifica 1, base_id viene impostato su-1.|  
|**magnitude**|**float**|Magnitude se viene utilizzato il tipo di codifica 1. Se non viene utilizzato il tipo di codifica 1, magnitude viene impostato su-1.|  
|**primary_dictionary_id**|**int**|Il valore 0 rappresenta il dizionario globale. Il valore-1 indica che non esiste alcun dizionario globale creato per questa colonna.|  
|**secondary_dictionary_id**|**int**|Un valore diverso da zero punta al dizionario locale per questa colonna nel segmento corrente (ad esempio, rowgroup). Il valore-1 indica che non esiste alcun dizionario locale per questo segmento.|  
|**min_data_id**|**bigint**|ID dati minimo nel segmento di colonna.|  
|**max_data_id**|**bigint**|ID dati massimo nel segmento di colonna.|  
|**null_value**|**bigint**|Valore utilizzato per rappresentare i valori Null.|  
|**on_disk_size**|**bigint**|Dimensioni del segmento in byte.|  
  
## <a name="remarks"></a>Commenti  
Il tipo di codifica del segmento columnstore viene selezionato da [!INCLUDE[ssde_md](../../includes/ssde_md.md)] con l'obiettivo di raggiungere il costo di archiviazione più basso, analizzando i dati del segmento. Se i dati sono per lo più distinti, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] Usa la codifica basata su valore. Se i dati non sono per lo più distinti, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] Usa la codifica basata su hash. La scelta tra la codifica basata su stringa e quella basata su valori è correlata al tipo di dati archiviati, a seconda che si tratti di dati stringa o binari. Tutte le codifiche sfruttano i vantaggi della codifica di bit e di lunghezza, quando possibile.
 
## <a name="permissions"></a>Autorizzazioni  
 Tutte le colonne richiedono almeno `VIEW DEFINITION` l'autorizzazione per la tabella. Le colonne seguenti restituiscono null a meno che l'utente non disponga anche delle `SELECT` autorizzazioni: `has_nulls` ,, `base_id` `magnitude` , `min_data_id` , `max_data_id` e `null_value` .  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  

## <a name="examples"></a>Esempio

### <a name="the-following-query-returns-information-about-segments-of-a-columnstore-index"></a>Nella query seguente vengono restituite le informazioni sui segmenti di un indice columnstore.  
  
```sql  
SELECT i.name, p.object_id, p.index_id, i.type_desc,   
    COUNT(*) AS number_of_segments  
FROM sys.column_store_segments AS s   
INNER JOIN sys.partitions AS p   
    ON s.hobt_id = p.hobt_id   
INNER JOIN sys.indexes AS i   
    ON p.object_id = i.object_id  
WHERE i.type = 5 OR i.type = 6  
GROUP BY i.name, p.object_id, p.index_id, i.type_desc ;  
GO  
```  

## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Domande frequenti sull'esecuzione di query sul catalogo di sistema SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.all_columns &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [sys.computed_columns &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-computed-columns-transact-sql.md)   
 [Guida agli indici columnstore](~/relational-databases/indexes/columnstore-indexes-overview.md)    
 [sys.column_store_dictionaries &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)  
  
 
