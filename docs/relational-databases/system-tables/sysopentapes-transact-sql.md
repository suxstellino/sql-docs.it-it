---
description: sysopentapes (Transact-SQL)
title: sysopentapes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysopentapes
- sysopentapes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- backup media [SQL Server], sysopentapes system table
- sysopentapes system table
ms.assetid: c066ca9b-9cfd-46b1-90a3-5c8dc9e7b6ae
author: cawrites
ms.author: chadam
ms.openlocfilehash: 133bbf06a4c25ba050287e83dcfd9005594483f4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195394"
---
# <a name="sysopentapes-transact-sql"></a>sysopentapes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una riga per ogni dispositivo nastro attualmente aperto. Questa vista è archiviata nel database **Master** .  
  
> [!IMPORTANT]  
>  Questa tabella di sistema è disponibile come vista per la compatibilità con le versioni precedenti. Utilizzare invece il sys.dm_io_backup_tapes &#40;la vista a gestione dinamica [&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-io-backup-tapes-transact-sql.md) .  
  
> [!NOTE]  
>  Non è possibile eliminare la visualizzazione **sysopentapes** .  

  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**openTape**|**nvarchar (64)**|Nome di file fisico del dispositivo nastro aperto. Per ulteriori informazioni sull'apertura e il rilascio di dispositivi nastro, vedere [BACKUP &#40;Transact-sql&#41;](../../t-sql/statements/backup-transact-sql.md) e [Restore &#40;transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).|  
  
## <a name="permissions"></a>Autorizzazioni  
 L'utente deve disporre dell'autorizzazione VIEW SERVER STATE nel server.  
  
  
