---
description: MSpeer_request (Transact-SQL)
title: MSpeer_request (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSpeer_request
- MSpeer_request_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSpeer_request system table
ms.assetid: ed048c46-7a2f-4ad0-bc7c-c2d65e83b4fb
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6bf63b8473efd3fd64b1ef19a0883c2cdb8e3f6c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098558"
---
# <a name="mspeer_request-transact-sql"></a>MSpeer_request (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella MSpeer_request viene utilizzata nella replica peer-to-peer per tenere traccia delle richieste di stato per una pubblicazione specificata. Questa tabella è archiviata nel database di pubblicazione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|id|**int**|Identifica una richiesta.|  
|pubblicazione|**sysname**|Nome della pubblicazione per cui è stata inviata la richiesta di stato.|  
|sent_date|**datetime**|Data e ora di invio della richiesta di stato.|  
|description|**nvarchar(4000)**|Informazioni specificate dall'utente che è possibile utilizzare per identificare le singole richieste di stato.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
