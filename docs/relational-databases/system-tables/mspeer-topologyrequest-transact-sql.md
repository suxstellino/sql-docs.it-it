---
description: MSpeer_topologyrequest (Transact-SQL)
title: MSpeer_topologyrequest (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSpeer_topologyrequest_TSQL
- MSpeer_topologyrequest
dev_langs:
- TSQL
helpviewer_keywords:
- MSpeer_topologyrequest
ms.assetid: c644814b-4e40-44d7-b6b4-5954b0d4db7c
author: cawrites
ms.author: chadam
ms.openlocfilehash: c343e25ecc6a05caa301dc8c8cc17ef4cacad1ca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205813"
---
# <a name="mspeer_topologyrequest-transact-sql"></a>MSpeer_topologyrequest (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Utilizzata nella replica peer-to-peer per tenere traccia delle richieste di stato di topologia per una pubblicazione. Questa tabella è archiviata nel database di pubblicazione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|id|**int**|Identifica una richiesta di stato di topologia. La colonna request_id in [MSpeer_topologyresponse](../../relational-databases/system-tables/mspeer-topologyresponse-transact-sql.md) utilizza questo valore.|  
|pubblicazione|**sysname**|Nome della pubblicazione da cui viene originata la richiesta di stato di topologia.|  
|sent_date|**datetime**|Data e ora di invio della richiesta di stato di topologia.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
