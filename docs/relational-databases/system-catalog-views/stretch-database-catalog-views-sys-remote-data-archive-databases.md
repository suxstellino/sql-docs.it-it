---
title: sys.remote_data_archive_databases (Transact-SQL) | Microsoft Docs
description: Informazioni su come sys.remote_data_archive_databases contiene una riga per ogni database remoto che archivia i dati da un database locale abilitato per l'estensione.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.remote_data_archive_databases
- sys.remote_data_archive_databases_TSQL
- remote_data_archive_databases
- remote_data_archive_databases_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.remote_data_archive_databases catalog view
ms.assetid: 25bffb0c-9821-40b4-88cf-75f854891a09
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
ms.openlocfilehash: 8ab3672838849626ea5f72f392ae102d8565b749
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185009"
---
# <a name="stretch-database-catalog-views---sysremote_data_archive_databases"></a>Viste del catalogo Stretch Database-sys.remote_data_archive_databases
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Contiene una riga per ogni database remoto che archivia i dati da un database locale abilitato per l'estensione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**remote_database_id**|**int**|Identificatore locale generato automaticamente del database remoto.|  
|**remote_database_name**|**sysname**|Nome del database remoto.|  
|**data_source_id**|**int**|Origine dati utilizzata per la connessione al server remoto|  
  
## <a name="see-also"></a>Vedere anche  
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)  
  
  
