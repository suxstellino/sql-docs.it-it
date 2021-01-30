---
description: sys.external_data_sources (Transact-SQL)
title: sys.external_data_sources (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 1016db6e-9950-4ae2-a004-bd4171e27359
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4854c6e7e7b02b127cd8622ecb3c089485da5464
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203836"
---
# <a name="sysexternal_data_sources-transact-sql"></a>sys.external_data_sources (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Contiene una riga per ogni origine dati esterna nel database corrente per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , [!INCLUDE[ssSDS](../../includes/sssds-md.md)] e [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] .  
  
 Contiene una riga per ogni origine dati esterna nel server per [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] .  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|data_source_id|**int**|ID oggetto per l'origine dati esterna.||  
|name|**sysname**|Nome dell'origine dati esterna.||  
|posizione|**nvarchar(4000)**|Stringa di connessione, che include il protocollo, l'indirizzo IP e la porta per l'origine dati esterna.||  
|type_desc|**nvarchar(255)**|Tipo di origine dati visualizzato sotto forma di stringa.|HADOOP, RDBMS, SHARD_MAP_MANAGER, RemoteDataArchiveTypeExtDataSource|  
|tipo|**tinyint**|Tipo di origine dati visualizzato sotto forma di numero.|0-HADOOP<br /><br /> 1-RDBMS<br /><br /> 2-SHARD_MAP_MANAGER<br /><br /> 3-RemoteDataArchiveTypeExtDataSource|  
|resource_manager_location|**nvarchar(4000)**|Per il tipo HADOOP, l'indirizzo IP e la posizione della porta di Hadoop Resource Manager. Viene usato per l'invio di un processo in un'origine dati Hadoop.<br /><br /> NULL per altri tipi di origini dati esterne.||  
|credential_id|**int**|ID oggetto della credenziale con ambito database utilizzata per la connessione all'origine dati esterna.||  
|database_name|**sysname**|Per il tipo RDBMS, il nome del database remoto. Per Type, SHARD_MAP_MANAGER, il nome del database di gestione delle mappe partizioni. NULL per altri tipi di origini dati esterne.||  
|shard_map_name|**sysname**|Per il tipo SHARD_MAP_MANAGER, il nome della mappa partizioni. NULL per altri tipi di origini dati esterne.||  
  
## <a name="permissions"></a>Autorizzazioni  
 La visibilità dei metadati nelle viste del catalogo è limitata alle entità a protezione diretta di cui l'utente è proprietario o per le quali dispone di autorizzazioni. Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [sys.external_file_formats &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-external-file-formats-transact-sql.md)   
 [sys.external_tables &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-external-tables-transact-sql.md)   
 [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md)  
  
  
