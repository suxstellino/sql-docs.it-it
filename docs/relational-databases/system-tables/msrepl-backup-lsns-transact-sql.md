---
description: MSrepl_backup_lsns (Transact-SQL)
title: MSrepl_backup_lsns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSrepl_backup_lsns_TSQL
- MSrepl_backup_lsns
dev_langs:
- TSQL
helpviewer_keywords:
- MSrepl_backup_Isns system table
ms.assetid: de06c349-82a8-48c6-b602-b5d6938514f6
author: cawrites
ms.author: chadam
ms.openlocfilehash: eb1f556b5b7a1373664f44aaaa1ff4608b96d2d8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098230"
---
# <a name="msrepl_backup_lsns-transact-sql"></a>MSrepl_backup_lsns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSrepl_backup_lsns** contiene i numeri di sequenza del log delle transazioni (LSN) per il supporto dell'opzione ' Sincronizza con backup ' del database di distribuzione. Questa tabella è archiviata nel database di distribuzione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**publisher_database_id**|**int**|ID del database del server di pubblicazione.|  
|**valid_xact_id**|**varbinary(16)**|ID della transazione da inviare al server di pubblicazione per contrassegnare il punto di troncamento del log. Viene utilizzato solo se il database di distribuzione è in modalità 'sync with backup'. Corrisponde all'ID dell'ultima transazione replicata nel database di distribuzione di cui è stato eseguito il backup. Viene inviato al server di pubblicazione per consentire all'agente di lettura log di contrassegnare il punto di troncamento del log.|  
|**valid_xact_seqno**|**varbinary(16)**|Numero di sequenza della transazione da inviare al server di pubblicazione per contrassegnare il punto di troncamento del log. Viene utilizzato solo se il database di distribuzione è in modalità 'sync with backup'. Corrisponde al numero di sequenza del file di log (LSN) dell'ultima transazione di replica nel database di distribuzione di cui è stato eseguito il backup. Viene inviato al server di pubblicazione per consentire all'agente di lettura log di contrassegnare il punto di troncamento del log.|  
|**next_xact_id**|**varbinary(16)**|Numero di sequenza del file di log (LSN) temporaneo utilizzato nelle operazioni di backup.|  
|**nextx_xact_seqno**|**varbinary(16)**|Numero di sequenza del file di log (LSN) temporaneo utilizzato nelle operazioni di backup.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
