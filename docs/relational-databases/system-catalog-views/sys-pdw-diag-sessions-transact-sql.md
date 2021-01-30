---
description: sys.pdw_diag_sessions (Transact-SQL)
title: sys.pdw_diag_sessions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 4d23688a-cddb-4eed-8231-ecde2a0b0e65
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: 2968fbd2fd9dc68b9130ec08225ecae7b268b3e1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203745"
---
# <a name="syspdw_diag_sessions-transact-sql"></a>sys.pdw_diag_sessions (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Contiene informazioni relative alle varie sessioni di diagnostica create nel sistema.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|  
|**nome**|**nvarchar(255)**|Nome della sessione di diagnostica.<br /><br /> Chiave per questa visualizzazione.||  
|**xml_data**|**nvarchar(4000)**|Payload XML che descrive la sessione.||  
|**is_active**|**bit**|Flag che indica se il flag è attivo.||  
|**host_address**|**nvarchar(255)**|Indirizzo del computer che ospita la definizione di sessione (nodo di controllo).||  
|**principal_id**|**int**|ID dell'utente che ha creato la sessione a livello di database.||  
|**database_id**|**int**|ID del database che rappresenta l'ambito della sessione di diagnostica.|  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo di Azure Synapse Analytics e Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  
