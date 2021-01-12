---
description: sys.index_columns (Transact-SQL)
title: sys.index_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.index_columns
- sys.index_columns_TSQL
- index_columns
- index_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.index_columns catalog view
ms.assetid: 211471aa-558a-475c-9b94-5913c143ed12
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cc5ce224b8010a5d49eef7f1be2f872c2685bd7a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094592"
---
# <a name="sysindex_columns-transact-sql"></a>sys.index_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contiene una riga per ogni colonna che fa parte di un indice **sys.** Indexes o di una tabella non ordinata (heap).  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID dell'oggetto su cui è definito l'indice.|  
|**index_id**|**int**|ID dell'indice nel quale è stata definita la colonna.|  
|**index_column_id**|**int**|ID della colonna dell'indice. **index_column_id** è univoco solo all'interno **index_id**.|  
|**column_id**|**int**|ID della colonna in **object_id**.<br /><br /> 0 = Identificatore di riga (RID, Row Identifier) in un indice non cluster.<br /><br /> **column_id** è univoco solo all'interno **object_id**.|  
|**key_ordinal**|**tinyint**|Numero ordinale (in base 1) nel set di colonne chiave.<br /><br /> 0 = la colonna non è una colonna chiave oppure è un indice XML, un indice columnstore o un indice spaziale.<br /><br /> Nota: un indice XML o spaziale non può essere una chiave perché le colonne sottostanti non sono confrontabili, vale a dire che i relativi valori non possono essere ordinati.|  
|**partition_ordinal**|**tinyint**|Numero ordinale (in base 1) nel set di colonne di partizionamento. In un indice columnstore cluster può essere presente al massimo 1 colonna di partizionamento.<br /><br /> 0 = La colonna non è una colonna di partizionamento.|  
|**is_descending_key**|**bit**|1 = Direzione di ordinamento decrescente per la colonna chiave dell'indice.<br /><br /> 0 = La colonna chiave dell'indice presenta una direzione di ordinamento crescente oppure la colonna fa parte di un indice hash.|  
|**is_included_column**|**bit**|1 = La colonna è una colonna non chiave aggiunta all'indice tramite la clausola CREATE INDEX INCLUDE oppure la colonna fa parte di un indice columnstore.<br /><br /> 0 = La colonna non è una colonna inclusa.<br /><br /> Le colonne aggiunte in modo implicito perché fanno parte della chiave di clustering non sono elencate in **sys.index_columns**.<br /><br /> Le colonne aggiunte in modo implicito perché colonne di partizionamento vengono restituite come 0.| 
|**column_store_order_ordinal**</br> Si applica a: Azure sinapsi Analytics (anteprima)|**tinyint**|Ordinale (in base 1) nel set di colonne ORDER in un indice columnstore cluster ordinato.|
  
## <a name="permissions"></a>Autorizzazioni

 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Esempi

 Nell'esempio seguente vengono restituiti tutti gli indici e le colonne degli indici della tabella `Production.BillOfMaterials`.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT i.name AS index_name  
    ,COL_NAME(ic.object_id,ic.column_id) AS column_name  
    ,ic.index_column_id  
    ,ic.key_ordinal  
,ic.is_included_column  
FROM sys.indexes AS i  
INNER JOIN sys.index_columns AS ic
    ON i.object_id = ic.object_id AND i.index_id = ic.index_id  
WHERE i.object_id = OBJECT_ID('Production.BillOfMaterials');  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
  
index_name                                                 column_name        index_column_id key_ordinal is_included_column  
---------------------------------------------------------- -----------------  --------------- ----------- -------------  
AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate ProductAssemblyID  1               1           0  
AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate ComponentID        2               2           0  
AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate StartDate          3               3           0  
PK_BillOfMaterials_BillOfMaterialsID                       BillOfMaterialsID  1               1           0  
IX_BillOfMaterials_UnitMeasureCode                         UnitMeasureCode    1               1           0  
  
(5 row(s) affected)  
  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [Domande frequenti sull'esecuzione di query sul catalogo di sistema di SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
