---
description: sys.dm_cluster_endpoints (Transact-SQL)
title: sys.dm_cluster_endpoints (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: database-engine, big-data-clusters
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_cluster_endpoints
- dm_cluster_endpoints_TSQL
- dm_cluster_endpoints
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_cluster_endpoints dynamic management view
ms.assetid: ''
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=sql-server-ver15||>=sql-server-linux-2017'
ms.openlocfilehash: 1d5d9cc9219422fd7559d73417354435eb20d488
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200347"
---
# <a name="sysdm_cluster_endpoints-transact-sql"></a>sys.dm_cluster_endpoints (Transact-SQL)
[!INCLUDE[sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]

|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|name|`sysname`|Nome del servizio esposto esternamente in un cluster SQL Big Data. Identificatore univoco per l'endpoint. Chiave per questa visualizzazione. Non ammette i valori Null. |  
|description|`nvarchar(4000)`|Descrizione del servizio Non ammette i valori Null. |
|endpoint|`sysname`|URL dell'endpoint o attributo di connessione. Non ammette i valori Null. |
|protocol_desc|`sysname`|Descrizione del protocollo dell'endpoint |

## <a name="permissions"></a>Autorizzazioni

In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.

## <a name="see-also"></a>Vedere anche

[Che cosa [!INCLUDE[big-data-clusters-2019](../../includes/ssbigdataclusters-ss-nover.md)] sono ](../../big-data-cluster/big-data-cluster-overview.md)?
