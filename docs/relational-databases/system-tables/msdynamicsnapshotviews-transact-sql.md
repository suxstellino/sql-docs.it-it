---
description: MSdynamicsnapshotviews (Transact-SQL)
title: MSdynamicsnapshotviews (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSdynamicsnapshotviews_TSQL
- MSdynamicsnapshotviews
dev_langs:
- TSQL
helpviewer_keywords:
- MSdynamicsnapshotviews system table
ms.assetid: 4fc1822a-5d6e-4034-a2e2-363210232d3b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 00561f1934d84c1c44b561ea62ff512ee9acf0f8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102081"
---
# <a name="msdynamicsnapshotviews-transact-sql"></a>MSdynamicsnapshotviews (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSdynamicsnapshotviews** tiene traccia di tutte le visualizzazioni snapshot dei dati filtrati temporanee create dall'agente snapshot e viene utilizzata dal sistema per la pulizia delle visualizzazioni in caso di arresto anomalo di SQL Server Agent o del agente di snapshot. Questa tabella è archiviata nei database di pubblicazione e di sottoscrizione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**dynamic_snapshot_view_name**|**sysname**|Nome della vista snapshot dei dati temporanei filtrati.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
