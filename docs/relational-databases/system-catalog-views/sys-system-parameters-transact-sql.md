---
description: sys.system_parameters (Transact-SQL)
title: sys.system_parameters (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.system_parameters
- sys.system_parameters_TSQL
- system_parameters_TSQL
- system_parameters
dev_langs:
- TSQL
helpviewer_keywords:
- sys.system_parameters catalog view
ms.assetid: 0d135c5f-68b5-4009-a0da-35e6abfee0ff
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c6337875c1cbed8aeacfed6cc78cedae98a7b3fb
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092994"
---
# <a name="syssystem_parameters-transact-sql"></a>sys.system_parameters (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contiene una riga per ogni oggetto di sistema con parametri.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID dell'oggetto a cui appartiene il parametro.|  
|**nome**|**sysname**|Nome del parametro. Valore univoco all'interno dell'oggetto.<br /><br /> Se l'oggetto è una funzione scalare, il nome del parametro è una stringa vuota nella riga che rappresenta il valore restituito.|  
|**parameter_id**|**int**|ID del parametro. Valore univoco all'interno dell'oggetto. Se l'oggetto è una funzione scalare, **parameter_id** = 0 rappresenta il valore restituito.|  
|**system_type_id**|**tinyint**|ID del tipo di sistema del parametro.|  
|**user_type_id**|**int**|ID del tipo di parametro definito dall'utente.<br /><br /> Per restituire il nome del tipo, eseguire il join alla vista del catalogo [sys. Types](../../relational-databases/system-catalog-views/sys-types-transact-sql.md) in questa colonna.|  
|**max_length**|**smallint**|Lunghezza massima del parametro, in byte. Il valore sarà-1 per quando il tipo di dati della colonna è **varchar (max)**, **nvarchar (max)**, **varbinary (max)** o **XML**.|  
|**precisione**|**tinyint**|Precisione del parametro se numerica. In caso contrario 0.|  
|**scale**|**tinyint**|Scala del parametro se numerica. In caso contrario 0.|  
|**is_output**|**bit**|1 = Parametro di output (o restituito), in caso contrario 0.|  
|**is_cursor_ref**|**bit**|1 = Il parametro è un parametro di riferimento a un cursore.|  
|**has_default_value**|**bit**|1 = il parametro ha un valore predefinito.<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta i valori predefiniti solo per gli oggetti CLR in questa vista del catalogo. Il valore di questa colonna sarà pertanto sempre 0 per gli oggetti [!INCLUDE[tsql](../../includes/tsql-md.md)]. Per visualizzare il valore predefinito di un parametro in un [!INCLUDE[tsql](../../includes/tsql-md.md)] oggetto, eseguire una query sulla colonna di **definizione** della vista del catalogo [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) oppure utilizzare la funzione di sistema [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md) .|  
|**is_xml_document**|**bit**|1 = Il contenuto è un documento XML completo.<br /><br /> 0 = il contenuto è un frammento di documento oppure il tipo di dati della colonna non è **XML**.|  
|**default_value**|**sql_variant**|Se **has_default_value** è 1, il valore di questa colonna corrisponde al valore predefinito per il parametro. in caso contrario, NULL.|  
|**xml_collection_id**|**int**|Diverso da zero se il tipo di dati del parametro è **XML** e il codice XML è tipizzato. Il valore corrisponde all'ID della raccolta contenente lo spazio dei nomi di convalida dell'XML Schema per il parametro.<br /><br /> 0 = Nessuna raccolta di XML Schema.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Domande frequenti sull'esecuzione di query sul catalogo di sistema SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [sys. Parameters &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)   
 [sys.all_parameters &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-all-parameters-transact-sql.md)  
  
  
