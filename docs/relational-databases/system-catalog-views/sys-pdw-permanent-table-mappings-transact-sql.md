---
title: sys.pdw_permanent_table_mappings (Transact-SQL)
description: Associa le tabelle utente permanenti ai nomi degli oggetti interni per **object_id**.
ms.custom: ''
ms.date: 07/24/2020
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
author: mstehrani
ms.author: emtehran
monikerRange: = azure-sqldw-latest
ms.openlocfilehash: 2c0a4c93443750a0b3bbe02797c376234ebd7570
ms.sourcegitcommit: 295b9dfc758471ef7d238a2b0f92f93e34acbb1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106054406"
---
# <a name="syspdw_permanent_table_mappings-transact-sql"></a>sys.pdw_permanent_table_mappings (Transact-SQL)
[!INCLUDE [applies-to-version/asa](../../includes/applies-to-version/asa.md)]

Associa le tabelle utente permanenti ai nomi degli oggetti interni per **object_id**.  
  
> [!NOTE]
> **sys.pdw_permanent_table_mappings** contiene i mapping alle tabelle permanenti e non include i mapping delle tabelle temporanee o esterne.

|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|physical_name|**nvarchar (36)**|Nome fisico della tabella.<br /><br /> **physical_name** e **object_id** formano la chiave per questa visualizzazione.|  
|object_id|**int**|ID oggetto per la tabella. Vedere [sys. objects &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).<br /><br /> **physical_name** e **object_id** formano la chiave per questa visualizzazione.|  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo di Azure Synapse Analytics e Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
 [sys.pdw_index_mappings &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-index-mappings-transact-sql.md)  
  
  
