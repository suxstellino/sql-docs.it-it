---
description: sys.allocation_units (Transact-SQL)
title: sys.allocation_units (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.allocation_units_TSQL
- sys.allocation_units
- allocation_units_TSQL
- allocation_units
dev_langs:
- TSQL
helpviewer_keywords:
- sys.allocation_units catalog view
ms.assetid: ec9de780-68fd-4551-b70b-2d3ab3709b3e
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 12a5924d1137f9face839899a742f20d3565f267
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091574"
---
# <a name="sysallocation_units-transact-sql"></a>sys.allocation_units (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Contiene una riga per ogni unità di allocazione nel database.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|allocation_unit_id|**bigint**|ID dell'unità di allocazione. Valore univoco all'interno di un database.|  
|type|**tinyint**|Tipo di unità di allocazione:<br /><br /> 0 = Rimossa<br /><br /> 1 = Dati all'interno di righe (tutti i tipi di dati, eccetto i tipi di dati LOB)<br /><br /> 2 = dati LOB (Large Object) (**Text**, **ntext**, **Image**, **XML**, tipi di valore di grandi dimensioni e tipi CLR definiti dall'utente)<br /><br /> 3 = Dati di overflow della riga|  
|type_desc|**nvarchar(60)**|Descrizione del tipo dell'unità di allocazione:<br /><br /> **ELIMINATO**<br /><br /> **IN_ROW_DATA**<br /><br /> **LOB_DATA**<br /><br /> **ROW_OVERFLOW_DATA**|  
|container_id|**bigint**|ID del contenitore di archiviazione associato all'unità di allocazione.<br /><br /> Se type = 1 o 3, container_id = sys.partitions.hobt_id.<br /><br /> Se type è 2, allora container_id = sys.partitions.partition_id.<br /><br /> 0 = Unità di allocazione contrassegnata per la rimozione posticipata|  
|data_space_id|**int**|ID del filegroup contenente l'unità di allocazione.|  
|total_pages|**bigint**|Numero totale di pagine allocate o riservate dall'unità di allocazione.|  
|used_pages|**bigint**|Numero totale di pagine effettivamente utilizzate.|  
|data_pages|**bigint**|Numero di pagine utilizzate contenenti:<br /><br /> Dati In-row<br /><br /> Dati LOB<br /><br /> Dati Row-overflow<br /><br /> <br /><br /> Si noti che il valore restituito esclude le pagine di indice interne e le pagine di gestione dell'allocazione.|  
  
> [!NOTE]  
>  In caso di eliminazione o ricompilazione di indici di grandi dimensioni oppure di eliminazione o troncamento di tabelle di grandi dimensioni, in [!INCLUDE[ssDE](../../includes/ssde-md.md)] le deallocazioni di pagine effettive e i relativi blocchi associati vengono posticipati fino all'esecuzione del commit della transazione. Le operazioni di eliminazione posticipate non rendono immediatamente disponibile lo spazio allocato. Pertanto, i valori restituiti da sys.allocation_units subito dopo l'eliminazione o il troncamento di un oggetto di grandi dimensioni potrebbero non corrispondere allo spazio su disco effettivamente disponibile.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** . Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
