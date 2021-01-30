---
description: sys.fn_MSxe_read_event_stream (Transact-SQL)
title: sys.fn_MSxe_read_event_stream (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_MSxe_read_event_stream_TSQL
- sys.fn_MSxe_read_event_stream_TSQL
- sys.fn_MSxe_read_event_stream
- fn_MSxe_read_event_stream
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fn_MSxe_read_event_stream
- fn_MSxe_read_event_stream
ms.assetid: 5edb1162-625a-41e0-8ec9-1edc8ab9a74a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: dc8eff18237f4796d36812cf0afee363effcbf99
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201926"
---
# <a name="sysfn_msxe_read_event_stream-transact-sql"></a>sys.fn_MSxe_read_event_stream (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gli eventi estesi forniscono una funzione con valori di tabella per l'utilizzo interno da parte dell'interfaccia utente degli eventi estesi. La tabella non fornisce dati utilizzabili dagli utenti.  
  
 Per visualizzare dati evento, utilizzare uno degli elementi seguenti:  
  
-   Interfaccia utente per una nuova sessione di eventi estesi.  
  
-   Funzione con valori di tabella fn_xe_file_target_read_file. Per altre informazioni, vedere [sys.fn_xe_file_target_read_file &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-xe-file-target-read-file-transact-sql.md).  
  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.fn_MSxe_read_event_stream ( session_name)  
```  
  
## <a name="arguments"></a>Argomenti  
 *Session_name*  
 Nome di una sessione eseguita nel server.  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|type|**Integer (4)**|Tipo di evento. Non ammette i valori Null.|  
|data|**Image (16)**|Dati immagine dell'evento. Ammette i valori Null.|  
  
## <a name="see-also"></a>Vedere anche  
 [Viste a gestione dinamica degli eventi estesi](../../relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views.md)   
 [Viste del catalogo degli eventi estesi &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)   
 [Eventi estesi](../../relational-databases/extended-events/extended-events.md)  
  
  
