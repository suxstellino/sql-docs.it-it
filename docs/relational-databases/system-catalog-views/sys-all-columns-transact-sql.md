---
description: sys.all_columns (Transact-SQL)
title: sys.all_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- all_columns_TSQL
- all_columns
- sys.all_columns_TSQL
- sys.all_columns
dev_langs:
- TSQL
helpviewer_keywords:
- sys.all_columns catalog view
ms.assetid: 40e04fe9-0b64-4799-84c0-57f128b2bdc2
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 030c3ce52f1707f71d007e5ece73b4fe92c7fd7a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102255"
---
# <a name="sysall_columns-transact-sql"></a>sys.all_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Visualizza l'unione di tutte le colonne appartenenti agli oggetti definiti dall'utente e agli oggetti di sistema.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|object_id|**int**|ID dell'oggetto a cui appartiene la colonna.|  
|name|**sysname**|Nome della colonna. Valore univoco all'interno dell'oggetto.|  
|column_id|**int**|ID della colonna. Valore univoco all'interno dell'oggetto.<br /><br /> È possibile che gli ID di colonna non siano sequenziali.|  
|system_type_id|**tinyint**|ID del tipo di sistema della colonna.|  
|user_type_id|**int**|ID del tipo di colonna definito dall'utente.<br /><br /> Per restituire il nome del tipo, eseguire il join alla vista del catalogo [sys. Types](../../relational-databases/system-catalog-views/sys-types-transact-sql.md) in questa colonna.|  
|max_length|**smallint**|Lunghezza massima in byte della colonna.<br /><br /> -1 = il tipo di dati della colonna è **varchar (max)**, **nvarchar (max)**, **varbinary (max)** o **XML**.<br /><br /> Per le colonne di **testo** , il valore max_length sarà 16 o il valore impostato da sp_tableoption ' text in row '.|  
|precisione|**tinyint**|Precisione della colonna se la colonna è di tipo numerico. In caso contrario, 0.|  
|scala|**tinyint**|Scala della colonna se la colonna è di tipo numerico. In caso contrario, 0.|  
|collation_name|**sysname**|Nome delle regole di confronto della colonna se la colonna è di tipo carattere. In caso contrario, NULL.|  
|is_nullable|**bit**|1 = La colonna ammette valori Null.|  
|is_ansi_padded|**bit**|1 = La colonna utilizza l'opzione ANSI_PADDING ON se è di tipo carattere, binary o variant.<br /><br /> 0 = La colonna non è di tipo carattere, binary o variant.|  
|is_rowguidcol|**bit**|1 = La colonna è una parola chiave ROWGUIDCOL dichiarata.|  
|is_identity|**bit**|1 = La colonna ha valori Identity|  
|is_computed|**bit**|1 = La colonna è una colonna calcolata.|  
|is_filestream|**bit**|1 = La colonna è stata dichiarata in modo che utilizzi l'archiviazione filestream.|  
|is_replicated|**bit**|1 = La colonna viene replicata.|  
|is_non_sql_subscribed|**bit**|1 = La colonna dispone di Sottoscrittore non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|is_merge_published|**bit**|1 = La colonna è inclusa in una pubblicazione di tipo merge.|  
|is_dts_replicated|**bit**|1 = La colonna viene replicata tramite [!INCLUDE[ssIS](../../includes/ssis-md.md)].|  
|is_xml_document|**bit**|1 = Il contenuto è un documento XML completo.<br /><br /> 0 = Il contenuto è un frammento di documento o la colonna non è di tipo XML.|  
|xml_collection_id|**int**|Diverso da zero se il tipo di dati della colonna è **XML** e il codice XML è tipizzato. Il valore sarà l'ID della raccolta contenente lo spazio dei nomi dell'XML Schema di convalida della colonna.<br /><br /> 0 = Nessuna raccolta di XML Schema.|  
|default_object_id|**int**|ID dell'oggetto predefinito, indipendentemente dal fatto che sia un [sys.sp_bindefault](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md)autonomo o un vincolo predefinito a livello di colonna inline. La colonna parent_object_id di un oggetto predefinito inline a livello di colonna è un riferimento alla tabella stessa.<br /><br /> 0 = nessun valore predefinito.|  
|rule_object_id|**int**|ID della regola autonoma associata alla colonna tramite sys.sp_bindrule.<br /><br /> 0 = Nessuna regola autonoma.<br /><br /> Per i vincoli CHECK a livello di colonna, vedere [sys.check_constraints &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md).|  
|is_sparse|bit|1 = La colonna è di tipo sparse. Per altre informazioni, vedere [Usare le colonne di tipo sparse](../../relational-databases/tables/use-sparse-columns.md).|  
|is_column_set|bit|1 = La colonna è un set di colonne. Per altre informazioni, vedere [Usare set di colonne](../../relational-databases/tables/use-column-sets.md).|  
|generated_always_type|**tinyint**|**Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] e versioni successive.<br /><br /> Valore numerico che rappresenta il tipo di colonna:<br /><br /> 0 = NOT_APPLICABLE<br /><br /> 1 = AS_ROW_START<br /><br /> 2 = AS_ROW_END|  
|generated_always_type_desc|**nvarchar(60)**|**Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] e versioni successive.<br /><br /> Descrizione testuale del tipo di colonna:<br /><br /> NOT_APPLICABLE<br /><br /> AS_ROW_START<br /><br /> AS_ROW_END|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Domande frequenti sull'esecuzione di query sul catalogo di sistema SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.system_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-system-columns-transact-sql.md)   
 [sys.computed_columns &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-computed-columns-transact-sql.md)  
  
  
