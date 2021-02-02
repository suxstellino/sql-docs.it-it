---
description: sys.dm_os_buffer_descriptors (Transact-SQL)
title: sys.dm_os_buffer_descriptors (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_buffer_descriptors_TSQL
- dm_os_buffer_descriptors_TSQL
- sys.dm_os_buffer_descriptors
- dm_os_buffer_descriptors
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_buffer_descriptors dynamic management view
ms.assetid: 012aab95-8888-4f35-9ea3-b5dff6e3f60f
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ce3894ac989047dc8274c6770e881066023ccabd
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250378"
---
# <a name="sysdm_os_buffer_descriptors-transact-sql"></a>sys.dm_os_buffer_descriptors (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce informazioni relative a tutte le pagine di dati incluse nel pool di buffer di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'output di questa vista può essere utilizzato per determinare la distribuzione delle pagine del database nel pool di buffer in base al database, all'oggetto o al tipo. In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] questa DMV restituisce inoltre informazioni sulle pagine di dati nel file di estensione del pool di buffer. Per altre informazioni, vedere [estensione del pool di buffer](../../database-engine/configure-windows/buffer-pool-extension.md).  
  
 Quando una pagina di dati viene letta dal disco, viene copiata nel pool di buffer di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e memorizzata nella cache per il riutilizzo. Ogni pagina di dati memorizzata nella cache è associata a un descrittore di buffer. I descrittori di buffer identificano in modo univoco ogni pagina di dati attualmente memorizzata nella cache in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. sys.dm_os_buffer_descriptors restituisce le pagine memorizzate nella cache per tutti i database utente e di sistema. incluse le pagine associate al database Resource.  
  
> **Nota:** Per chiamare questo [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oggetto da o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , usare il nome **sys.dm_pdw_nodes_os_buffer_descriptors**.  

|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|database_id|**int**|ID del database associato alla pagina nel pool di buffer. Ammette i valori Null.|  
|file_id|**int**|ID del file in cui è archiviata l'immagine persistente della pagina. Ammette i valori Null.|  
|page_id|**int**|ID della pagina all'interno del file. Ammette i valori Null.|  
|page_level|**int**|Livello di indice della pagina. Ammette i valori Null.|  
|allocation_unit_id|**bigint**|ID dell'unità di allocazione della pagina. Questo valore può essere utilizzato per unire in join sys.allocation_units. Ammette i valori Null.|  
|page_type|**nvarchar(60)**|Tipo di pagina, ad esempio pagina di dati o pagina di indice. Ammette i valori Null.|  
|row_count|**int**|Numero di righe nella pagina. Ammette i valori Null.|  
|free_space_in_bytes|**int**|Quantità di spazio disponibile, in byte, nella pagina. Ammette i valori Null.|  
|is_modified|**bit**|1 = La pagina è stata modificata dopo essere stata letta dal disco. Ammette i valori Null.|  
|numa_node|**int**|Nodo NUMA (non-uniform memory access) per il buffer. Ammette i valori Null.|  
|read_microsec|**bigint**|Tempo effettivo (in microsecondi) necessario per leggere la pagina nel buffer. Questo numero viene reimpostato quando si riutilizza il buffer. Ammette i valori Null.|  
|is_in_bpool_extension|**bit**|1 = la pagina è nell'estensione del pool di buffer. Ammette i valori Null.|  
|pdw_node_id|**int**|**Si applica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificatore del nodo su cui si trova questa distribuzione.|  
  
## <a name="permissions"></a>Autorizzazioni  

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, il `Server admin` o un `Azure Active Directory admin` account è obbligatorio. Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   
   
## <a name="remarks"></a>Commenti  
 sys.dm_os_buffer_descriptors restituisce le pagine utilizzate dal database Resource. sys.dm_os_buffer_descriptors non restituisce informazioni sulle pagine libere o rubate oppure sulle pagine che contengono errori durante la lettura.  
  
|Da|A|On|Relationship|  
|----------|--------|--------|------------------|  
|sys.dm_os_buffer_descriptors|sys.databases|database_id|molti-a-uno|  
|sys.dm_os_buffer_descriptors|\<userdb>.sys.allocation_units|allocation_unit_id|molti-a-uno|  
|sys.dm_os_buffer_descriptors|\<userdb>.sys.database_files|file_id|molti-a-uno|  
|sys.dm_os_buffer_descriptors|sys.dm_os_buffer_pool_extension_configuration|file_id|molti-a-uno|  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-returning-cached-page-count-for-each-database"></a>R. Restituzione del conteggio delle pagine memorizzate nella cache per ogni database  
 Nell'esempio seguente viene restituito il conteggio delle pagine caricate per ogni database.  
  
```  
SELECT COUNT(*)AS cached_pages_count  
    ,CASE database_id   
        WHEN 32767 THEN 'ResourceDb'   
        ELSE db_name(database_id)   
        END AS database_name  
FROM sys.dm_os_buffer_descriptors  
GROUP BY DB_NAME(database_id) ,database_id  
ORDER BY cached_pages_count DESC;  
```  
  
### <a name="b-returning-cached-page-count-for-each-object-in-the-current-database"></a>B. Restituzione del conteggio delle pagine memorizzate nella cache per ogni oggetto nel database corrente  
 Nell'esempio seguente viene restituito il conteggio delle pagine caricate per ogni oggetto nel database corrente.  
  
```  
SELECT COUNT(*)AS cached_pages_count   
    ,name ,index_id   
FROM sys.dm_os_buffer_descriptors AS bd   
    INNER JOIN   
    (  
        SELECT object_name(object_id) AS name   
            ,index_id ,allocation_unit_id  
        FROM sys.allocation_units AS au  
            INNER JOIN sys.partitions AS p   
                ON au.container_id = p.hobt_id   
                    AND (au.type = 1 OR au.type = 3)  
        UNION ALL  
        SELECT object_name(object_id) AS name     
            ,index_id, allocation_unit_id  
        FROM sys.allocation_units AS au  
            INNER JOIN sys.partitions AS p   
                ON au.container_id = p.partition_id   
                    AND au.type = 2  
    ) AS obj   
        ON bd.allocation_unit_id = obj.allocation_unit_id  
WHERE database_id = DB_ID()  
GROUP BY name, index_id   
ORDER BY cached_pages_count DESC;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sys.allocation_units &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)   
 
 [SQL Server viste a gestione dinamica relative al sistema operativo &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [Database Resource](../../relational-databases/databases/resource-database.md)   
 [sys.dm_os_buffer_pool_extension_configuration &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-buffer-pool-extension-configuration-transact-sql.md)  
  
  


