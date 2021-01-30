---
description: sys.external_tables (Transact-SQL)
title: sys.external_tables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: fac4720c-b679-4ab2-864b-ff7810a9b559
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: caa4ea9fa1ae72760652912b3001f40e7c9e0c09
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193969"
---
# <a name="sysexternal_tables-transact-sql"></a>sys.external_tables (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Contiene una riga per ogni tabella esterna nel database corrente.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|\<inherited columns>||Per un elenco di colonne ereditate da questa vista, vedere [sys. objects &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).||  
|max_column_id_used|**int**|ID di colonna massimo utilizzato per la tabella.||  
|uses_ansi_nulls|**bit**|La tabella è stata creata con l'opzione di database SET ANSI_NULLS impostata su ON.||  
|data_source_id|**int**|ID oggetto per l'origine dati esterna.||  
|file_format_id|**int**|Per le tabelle esterne in un'origine dati esterna HADOOP, si tratta dell'ID oggetto per il formato di file esterno.||  
|posizione|**nvarchar(4000)**|Per le tabelle esterne in un'origine dati esterna HADOOP, questo è il percorso dei dati esterni in HDFS.||  
|reject_type|**tinyint**|Per le tabelle esterne in un'origine dati esterna HADOOP, questo è il modo in cui vengono conteggiate le righe rifiutate durante l'esecuzione di query su dati esterni.|VALUE: numero di righe rifiutate.<br /><br /> PERCENTUALE: la percentuale di righe rifiutate.|  
|reject_value|**float**|Per le tabelle esterne in un'origine dati esterna HADOOP:<br /><br /> Per *reject_type =* value, indica il numero di rifiuti di riga da consentire prima che la query abbia esito negativo.<br /><br /> Per *reject_type* = percentuale, indica la percentuale di rifiuti di riga da consentire prima che la query abbia esito negativo.||  
|reject_sample_value|**int**|Per *reject_type* = percentuale, indica il numero di righe da caricare, con esito positivo o negativo, prima di calcolare la percentuale di righe rifiutate.|NULL se reject_type = VALUE.|  
|distribution_type|**int**|Per le tabelle esterne su un'origine dati esterna SHARD_MAP_MANAGER, si tratta della distribuzione dei dati delle righe nelle tabelle di base sottostanti.|0-partizionato<br /><br /> 1-replicata<br /><br /> 2-Round Robin|  
|distribution_desc|**nvarchar(120)**|Per le tabelle esterne su un'origine dati esterna SHARD_MAP_MANAGER, questo è il tipo di distribuzione visualizzato sotto forma di stringa.||  
|sharding_column_id|**int**|Per le tabelle esterne su un'origine dati esterna SHARD_MAP_MANAGER e una distribuzione partizionata, si tratta dell'ID di colonna della colonna che contiene i valori delle chiavi di partizionamento orizzontale.||  
|remote_schema_name|**sysname**|Per le tabelle esterne su un'origine dati esterna SHARD_MAP_MANAGER, si tratta dello schema in cui si trova la tabella di base nei database remoti, se diverso dallo schema in cui è definita la tabella esterna.||  
|remote_object_name|**sysname**|Per le tabelle esterne su un'origine dati esterna SHARD_MAP_MANAGER, questo è il nome della tabella di base nei database remoti, se diverso dal nome della tabella esterna.||  
  
## <a name="permissions"></a>Autorizzazioni  
 La visibilità dei metadati nelle viste del catalogo è limitata alle entità a protezione diretta di cui l'utente è proprietario o per le quali dispone di autorizzazioni. Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [sys.external_file_formats &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-external-file-formats-transact-sql.md)   
 [sys.external_data_sources &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-external-data-sources-transact-sql.md)   
 [CREATE EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-transact-sql.md)  
  
  
