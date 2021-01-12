---
description: sys.computed_columns (Transact-SQL)
title: sys.computed_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.computed_columns_TSQL
- sys.computed_columns
- computed_columns_TSQL
- computed_columns
dev_langs:
- TSQL
helpviewer_keywords:
- sys.computed_columns catalog view
ms.assetid: c962c619-e18f-4315-9251-8d9862462299
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a7dbe7e9176605dea875e1846b72e1f00301be55
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100706"
---
# <a name="syscomputed_columns-transact-sql"></a>sys.computed_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contiene una riga per ogni colonna trovata in **sys. Columns** che è una colonna calcolata.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**\<Inherited columns>**||La vista **sys.computed_columns** restituisce tutte le colonne della vista **sys. Columns** . Vengono inoltre restituite le colonne aggiuntive descritte di seguito. Per una descrizione delle colonne ereditate dalla vista **sys.computed_columns** da **sys. Columns**, vedere [sys. Columns &#40;&#41;Transact-SQL](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md). Il valore della colonna **is_computed** è sempre impostato su 1 nella visualizzazione **sys.computed_columns** .|  
|**definizione**|**nvarchar(max)**|Testo SQL che definisce la colonna calcolata.|  
|**uses_database_collation**|**bit**|1 = La definizione della colonna dipende dalle regole di confronto predefinite del database; altrimenti, 0. Tale dipendenza impedisce la modifica delle regole di confronto predefinite del database.|  
|**is_persisted**|**bit**|La colonna calcolata è persistente.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
