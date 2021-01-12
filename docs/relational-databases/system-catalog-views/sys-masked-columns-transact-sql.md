---
description: sys.masked_columns (Transact-SQL)
title: sys.masked_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/25/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.masked_columns
- masked_columns_tsql
- sys.masked_columns_tsql
- masked_columns
helpviewer_keywords:
- sys.masked_columns catalog view
ms.assetid: 671577e4-d757-4b8d-9aa9-0fc8d51ea9ca
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e5096f57c8e0b06c1f0403c446eb5417cd13b6f1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095552"
---
# <a name="sysmasked_columns-transact-sql"></a>sys.masked_columns (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Utilizzare la vista **sys.masked_columns** per eseguire una query per le colonne della tabella a cui è applicata una funzione di maschera dati dinamica. Questa vista viene ereditata dalla vista **sys.columns** , che restituisce tutte le colonne della vista **sys.columns** , in aggiunta alle colonne **is_masked** e **masking_function** , che indicano se la colonna è nascosta e, in tal caso, quale funzione di maschera viene definita. Questa vista mostra solo le colonne su cui è applicata una funzione di maschera.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|object_id|**int**|ID dell'oggetto a cui appartiene la colonna.|  
|name|**sysname**|Nome della colonna. Valore univoco all'interno dell'oggetto.|  
|column_id|**int**|ID della colonna. Valore univoco all'interno dell'oggetto.<br /><br /> È possibile che gli ID di colonna non siano sequenziali.|  
|**sys.masked_columns** restituisce molte più colonne ereditate da **sys. Columns**.|variabile|Per altre definizioni di colonna, vedere [sys. columns &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md) .|  
|is_masked|**bit**|Indica se la colonna è mascherata. 1 indica mascherata.|  
|masking_function|**nvarchar(4000)**|Funzione di maschera per la colonna.|  
  
## <a name="remarks"></a>Osservazioni  
  
## <a name="permissions"></a>Autorizzazioni  
 In questa vista vengono restituite informazioni sulle tabelle in cui l'utente dispone di una sorta di autorizzazione per la tabella o se l'utente dispone dell'autorizzazione VIEW ANY DEFINITION.  
  
## <a name="example"></a>Esempio  
 La query seguente consente di unire **sys.masked_columns** a **sys. Tables** per restituire informazioni su tutte le colonne mascherate.  
  
```  
SELECT tbl.name as table_name, c.name AS column_name, c.is_masked, c.masking_function  
FROM sys.masked_columns AS c  
JOIN sys.tables AS tbl   
    ON c.object_id = tbl.object_id  
WHERE is_masked = 1;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Dynamic Data Masking](../../relational-databases/security/dynamic-data-masking.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)  
  
  
