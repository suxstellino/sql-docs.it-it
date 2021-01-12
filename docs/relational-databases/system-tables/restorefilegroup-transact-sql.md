---
description: restorefilegroup (Transact-SQL)
title: restorefilegroup (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- restorefilegroup_TSQL
- restorefilegroup
dev_langs:
- TSQL
helpviewer_keywords:
- filegroups [SQL Server], restorefilegroup system table
- restorefilegroup system table
ms.assetid: 3aa15c55-6b72-4f76-97d7-bd88391d105c
author: cawrites
ms.author: chadam
ms.openlocfilehash: b4be126afb404fcff25dfa5ce036a70189d7b640
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096171"
---
# <a name="restorefilegroup-transact-sql"></a>restorefilegroup (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una riga per ogni filegroup ripristinato. Questa tabella è archiviata nel database **msdb** .  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**restore_history_id**|**int**|Numero di identificazione univoco che identifica l'operazione di ripristino corrispondente. Fa riferimento a **restorehistory (restore_history_id)**.|  
|**filegroup_name**|**nvarchar(128)**|Nome del filegroup ripristinato. Può essere NULL.<br /><br /> Quando un database viene ripristinato come snapshot di database, questo valore viene popolato nello stesso modo previsto per un ripristino completo.|  
  
## <a name="remarks"></a>Osservazioni  
 Per ridurre il numero di righe in questa tabella e in altre tabelle di backup e di cronologia, eseguire la [sp_delete_backuphistory](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md) stored procedure.  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di backup e ripristino &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [restorefile &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/restorefile-transact-sql.md)   
 [restorehistory &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/restorehistory-transact-sql.md)   
 [Tabelle di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
