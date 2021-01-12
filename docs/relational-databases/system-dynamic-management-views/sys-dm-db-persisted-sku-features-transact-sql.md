---
description: sys.dm_db_persisted_sku_features (Transact-SQL)
title: sys.dm_db_persisted_sku_features (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_persisted_sku_features_TSQL
- sys.dm_db_persisted_sku_features
- dm_db_persisted_sku_features_TSQL
- dm_db_persisted_sku_features
dev_langs:
- TSQL
helpviewer_keywords:
- editions [SQL Server]
- sys.dm_db_persisted_sku_features dynamic management view
ms.assetid: b4b29e97-b523-41b9-9528-6d4e84b89e09
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9221515e0c8a6c0704f0a036d851241d849d5f3d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101669"
---
# <a name="sysdm_db_persisted_sku_features-transact-sql"></a>sys.dm_db_persisted_sku_features (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Alcune funzionalità del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] comportano una modifica del metodo di archiviazione delle informazioni nei file di database da parte del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Queste funzionalità sono disponibili solo in edizioni specifiche di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un database che contiene queste funzionalità non può essere spostato a un'edizione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che non le supporta. Utilizzare la vista a gestione dinamica sys.dm_db_persisted_sku_features per elencare le funzionalità specifiche dell'edizione abilitate nel database corrente.
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive).
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|feature_name|**sysname**|Nome esterno della funzionalità abilitata nel database ma non supportata in tutte le edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa funzionalità deve essere rimossa prima di poter eseguire la migrazione del database in tutte le edizioni disponibili di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|feature_id|**int**|ID funzionalità associato alla funzionalità. [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)].|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW DATABASE STATE per il database.  
  
## <a name="remarks"></a>Osservazioni  
 Se nel database non sono utilizzate funzionalità che possono essere limitate da un'edizione specifica, la vista non restituisce alcuna riga.  
  
 sys.dm_db_persisted_sku_features possibile elencare le funzionalità di modifica del database seguenti come limitate a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] edizioni specifiche:  
  
-   **ChangeCapture**: indica che Change Data Capture è abilitato un database. Per rimuovere Change Data Capture, utilizzare il stored procedure [sys.sp_cdc_disable_db](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-db-transact-sql.md) . Per altre informazioni, vedere [Informazioni su Change Data Capture &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md).  
  
-   **ColumnStoreIndex**: indica che almeno una tabella include un indice columnstore. Per consentire lo spostamento di un database in un'edizione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che non supporta questa funzionalità, utilizzare l'istruzione [Drop index](../../t-sql/statements/drop-index-transact-sql.md) o [alter index](../../t-sql/statements/alter-index-transact-sql.md) per rimuovere l'indice columnstore. Per altre informazioni, vedere [indici columnstore](../../relational-databases/indexes/columnstore-indexes-overview.md).  
  
    **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive).  
  
-   **Compression**: indica che almeno una tabella o un indice utilizza la compressione dei dati o il formato di archiviazione vardecimal. Per consentire lo spostamento di un database in un'edizione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che non supporta questa funzionalità, utilizzare l'istruzione [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) o [alter index](../../t-sql/statements/alter-index-transact-sql.md) per rimuovere la compressione dei dati. Per rimuovere il formato di archiviazione vardecimal, utilizzare l'istruzione sp_tableoption. Per altre informazioni, vedere [Data Compression](../../relational-databases/data-compression/data-compression.md).  
  
-   **MultipleFSContainers**: indica che il database utilizza più contenitori FILESTREAM. Il database dispone di un filegroup FILESTREAM con più contenitori (file). Per altre informazioni, vedere [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md).  
  
-   **InMemoryOLTP**: indica che il database usa In-Memory OLTP. Al database è associato il filegroup MEMORY_OPTIMIZED_DATA. Per altre informazioni, vedere [OLTP in memoria &#40;ottimizzazione in memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
  **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive). 
  
-   **Partizionamento.** Indica che il database contiene tabelle partizionate, indici partizionati, schemi di partizione o funzioni di partizione. Per consentire di spostare un database in un'edizione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] diversa da Enterprise o Developer, non è sufficiente modificare la tabella affinché sia inclusa in una singola partizione, ma è necessario rimuovere la tabella partizionata. Se la tabella contiene dati, utilizzare SWITCH PARTITION per convertire ogni partizione in una tabella non partizionata. Eliminare quindi la tabella partizionata, lo schema di partizione e la funzione di partizione.  
  
-   **TransparentDataEncryption.** Indica che un database viene crittografato utilizzando Transparent Data Encryption. Per rimuovere Transparent Data Encryption, utilizzare l'istruzione ALTER DATABASE. Per altre informazioni sulla crittografia trasparente del database, vedere [Transparent Data Encryption &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md).  

> [!NOTE]
> A partire da [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] Service Pack 1, queste funzionalità, ad eccezione di **TransparentDataEncryption.** sono disponibili in più [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] edizioni e non solo per le edizioni Enterprise o Developer.

 Per determinare se in un database sono in uso funzionalità disponibili solo in edizioni specifiche, eseguire l'istruzione seguente nel database:  
  
```sql  
SELECT feature_name FROM sys.dm_db_persisted_sku_features;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Viste a gestione dinamica relative ai database &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
 [Edizioni e le funzionalità supportate di SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md)   
 [Edizioni e le funzionalità supportate di SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md)  
  
