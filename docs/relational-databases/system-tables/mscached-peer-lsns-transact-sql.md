---
description: MScached_peer_lsns (Transact-SQL)
title: MScached_peer_lsns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MScached_peer_lsns_TSQL
- MScached_peer_lsns
dev_langs:
- TSQL
helpviewer_keywords:
- MScached_peer_lsns system table
ms.assetid: f8b6089a-0230-45f9-8c34-9fe0d2a3a74e
author: cawrites
ms.author: chadam
ms.openlocfilehash: f1b9c527f1411f84afeef72d60c266ef1f3a91ea
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092425"
---
# <a name="mscached_peer_lsns-transact-sql"></a>MScached_peer_lsns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MScached_peer_lsns** viene utilizzata per tenere traccia dei valori LSN nel log delle transazioni utilizzati per determinare i comandi da restituire a un determinato sottoscrittore nella replica peer-to-peer. Questa tabella è archiviata nel database di distribuzione.  
  
## <a name="definition"></a>Definizione  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**agent_id**|**int**|ID dell'agente di distribuzione.|  
|**Creatore**|**sysname**|Nome del server di pubblicazione di origine.|  
|**originator_db**|**sysname**|Nome del database di pubblicazione di origine.|  
|**originator_publication_id**|**int**|Identifica la pubblicazione di origine.|  
|**originator_db_version**|**int**|Identifica il numero di versione del database di origine.|  
|**originator_lsn**|**varbinary(16)**|LSN della transazione di origine.|  
  
## <a name="remarks"></a>Osservazioni  
 Gli LSN vengono utilizzati solo immediatamente dopo l'inserimento e non hanno un significato permanente nel sistema.  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
