---
description: systranschemas (Transact-SQL)
title: systranschemas (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- systranschemas
- systranschemas_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- systranschemas system table
ms.assetid: 864c3966-cb61-4f2b-8939-ccda112de853
author: stevestein
ms.author: sstein
ms.openlocfilehash: 78242bbe5df333aa23dfd2b8c22688ef5e2e6284
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181541"
---
# <a name="systranschemas-transact-sql"></a>systranschemas (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **systranschemas** viene utilizzata per tenere traccia delle modifiche dello schema negli articoli pubblicati nelle pubblicazioni transazionali e snapshot. Questa tabella è archiviata nei database di pubblicazione e di sottoscrizione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**tabid**|**int**|Identificatore dell'articolo di tabella nel quale è stata apportata la modifica dello schema.|  
|**StartLsn**|**binary**|Valore LSN all'inizio della modifica dello schema.|  
|**endlsn**|**binary**|Valore LSN alla fine della modifica dello schema.|  
|**TypeId**|**int**|Tipo di modifica dello schema.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
