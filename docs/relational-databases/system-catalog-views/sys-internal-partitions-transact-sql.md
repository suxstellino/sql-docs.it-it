---
description: sys.internal_partitions (Transact-SQL)
title: sys.internal_partitions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 0262df2b-5ba7-4715-b17b-3d9ce470a38e
author: ronortloff
ms.author: rortloff
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 402e8ff5793ad0ea5abd11cad60f93b6ae963df2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191410"
---
# <a name="sysinternal_partitions-transact-sql"></a>sys.internal_partitions (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Restituisce una riga per ogni set di righe che tiene traccia dei dati interni per gli indici columnstore nelle tabelle basate su disco. Questi set di righe sono interni agli indici columnstore e tengono traccia delle righe eliminate, dei mapping rowgroup e dei RowGroups di archiviazione Delta. Consentono di tenere traccia dei dati per ogni partizione della tabella. ogni tabella include almeno una partizione. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ricrea i set di righe ogni volta che viene ricompilato l'indice columnstore.   
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|partition_id|**bigint**|ID partizione per la partizione. Valore univoco all'interno di un database.|  
|object_id|**int**|ID oggetto per la tabella che contiene la partizione.|  
|index_id|**int**|ID di indice per l'indice columnstore definito nella tabella.<br /><br /> 1 = indice columnstore cluster<br /><br /> 2 = indice columnstore non cluster|  
|partition_number|**int**|Numero di partizione.<br /><br /> 1 = prima partizione di una tabella partizionata o singola partizione di una tabella non partizionata.<br /><br /> 2 = seconda partizione e così via.|  
|internal_object_type|**tinyint**|Oggetti set di righe che tengono traccia dei dati interni per l'indice columnstore.<br /><br /> 2 = COLUMN_STORE_DELETE_BITMAP<br /><br /> 3 = COLUMN_STORE_DELTA_STORE<br /><br /> 4 = COLUMN_STORE_DELETE_BUFFER<br /><br /> 5 = COLUMN_STORE_MAPPING_INDEX|  
|internal_object_type_desc|**nvarchar(60)**|COLUMN_STORE_DELETE_BITMAP: questo indice bitmap tiene traccia delle righe contrassegnate come eliminate dal columnstore. La bitmap è per ogni rowgroup poiché le partizioni possono avere righe in più RowGroups. Le righe sono ancora fisicamente presenti e occupano spazio nel columnstore.<br /><br /> COLUMN_STORE_DELTA_STORE-archivia gruppi di righe, denominati RowGroups, che non sono stati compressi nell'archiviazione a colonne. Ogni partizione di tabella può avere zero o più RowGroups deltastore.<br /><br /> COLUMN_STORE_DELETE_BUFFER: per la gestione delle eliminazioni in indici columnstore non cluster aggiornabili. Quando una query elimina una riga dalla tabella rowstore sottostante, il buffer di eliminazione tiene traccia dell'eliminazione dal columnstore. Quando il numero di righe eliminate supera 1048576, viene eseguito il merge nella bitmap DELETE tramite il thread del motore di tuple in background o mediante un comando esplicito REORGANIZE.  In qualsiasi momento, l'Unione della bitmap Delete e del buffer Delete rappresenta tutte le righe eliminate.<br /><br /> COLUMN_STORE_MAPPING_INDEX: utilizzato solo quando l'indice columnstore cluster dispone di un indice non cluster secondario. Questa operazione esegue il mapping delle chiavi di indice non cluster al rowgroup e all'ID di riga corretti nel columnstore. Archivia solo le chiavi per le righe che si spostano in un rowgroup diverso; Questo errore si verifica quando un rowgroup Delta viene compresso nel columnstore e quando un'operazione di Unione unisce le righe di due RowGroups diversi.|  
|Row_group_id|**int**|ID per il rowgroup deltastore. Ogni partizione di tabella può avere zero o più RowGroups deltastore.|  
|hobt_id|**bigint**|ID dell'oggetto set di righe interno (HoBT). Si tratta di una chiave efficace per l'Unione con altri DMV per ottenere ulteriori informazioni sulle caratteristiche fisiche del set di righe interno.|  
|rows|**bigint**|Numero approssimativo di righe nella partizione.|  
|data_compression|**tinyint**|Stato di compressione per il set di righe:<br /><br /> 0 = NONE<br /><br /> 1 = ROW<br /><br /> 2 = PAGE|  
|data_compression_desc|**nvarchar(60)**|Stato di compressione per ogni partizione. I valori possibili per le tabelle rowstore sono NONE, ROW e PAGE. I valori possibili per le tabelle columnstore sono COLUMNSTORE e COLUMNSTORE_ARCHIVE.|  
|optimize_for_sequential_key|**bit**|1 = per la partizione è abilitata l'ottimizzazione dell'inserimento dell'ultima pagina.<br><br>0 = valore predefinito. Per la partizione è stata disabilitata l'ottimizzazione dell'inserimento dell'ultima pagina.|
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede l'appartenenza al ruolo `public`. Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="general-remarks"></a>Osservazioni generali  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ricrea nuovi indici interni columnstore ogni volta che crea o ricompila un indice columnstore.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-view-all-of-the-internal-rowsets-for-a-table"></a>R. Visualizzare tutti i set di righe interni per una tabella  
 In questo esempio vengono restituiti tutti i set di righe columnstore interni per una tabella. È inoltre possibile utilizzare il hobt_id per trovare ulteriori informazioni sul set di righe specifico.  
  
```sql  
SELECT i.object_id, i.index_id, i.name, p.hobt_id, p.internal_object_type_id, p.internal_object_type_desc  
FROM sys.internal_partitions AS p  
JOIN sys.indexes AS i  
on i.object_id = p.object_id  
WHERE p.object_id = OBJECT_ID ( '<table name' ) ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Domande frequenti sull'esecuzione di query sul catalogo di sistema di SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
