---
title: backupmediafamily (Transact-SQL) | Microsoft Docs
description: Riferimento per backupmediafamily, che contiene una riga per ogni gruppo di supporti.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- backupmediafamily
- backupmediafamily_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- backupmediafamily system table
- backup media [SQL Server], backupmediafamily system table
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6adacacdb3e075e3eb058005d3b11fc8fc219cbe
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096289"
---
# <a name="backupmediafamily-transact-sql"></a>backupmediafamily (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Include una riga per ogni gruppo di supporti. Se un gruppo di supporti risiede in un set di supporti con mirroring, il gruppo includerà una riga distinta per ciascun mirror del set di supporti. Questa tabella è archiviata nel database **msdb** .  
    
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**media_set_id**|**int**|Numero di identificazione univoco del set di supporti in cui è incluso il gruppo. Fa riferimento a **BackupMediaSet (media_set_id)**|  
|**family_sequence_number**|**tinyint**|Posizione del gruppo di supporti nel set di supporti.|  
|**media_family_id**|**uniqueidentifier**|Numero di identificazione univoco del gruppo di supporti. Può essere NULL.|  
|**media_count**|**int**|Numero di supporti nel gruppo. Può essere NULL.|  
|**logical_device_name**|**nvarchar(128)**|Nome del dispositivo di backup in **sys.backup_devices. Name**. Se si tratta di un dispositivo di backup temporaneo (in contrapposizione a un dispositivo di backup permanente presente in **sys.backup_devices**), il valore di **logical_device_name** è null.|  
|**physical_device_name**|**nvarchar(260)**|Nome fisico del dispositivo di backup. Può essere NULL. Questo campo è condiviso tra il processo di backup e ripristino. Può contenere il percorso di destinazione del backup originale o il percorso di origine del ripristino originale. A seconda del fatto che il backup o il ripristino si sia verificato per primo in un server per un database. Si noti che i ripristini consecutivi dallo stesso file di backup non aggiorneranno il percorso indipendentemente dalla posizione in fase di ripristino. Per questo motivo, non è possibile usare **physical_device_name** campo per visualizzare il percorso di ripristino usato.|  
|**device_type**|**tinyint**|Tipo di dispositivo di backup:<br /><br /> 2 = Disco<br /><br /> 5 = Nastro<br /><br /> 7 = Dispositivo virtuale<br /><br /> 9 = archiviazione di Azure<br /><br /> 105 = Dispositivo di backup permanente<br /><br /> Può essere NULL.<br /><br /> Tutti i nomi e i numeri di dispositivo permanenti sono disponibili in **sys.backup_devices**.|  
|**physical_block_size**|**int**|Dimensioni fisiche del blocco utilizzate per la scrittura del gruppo di supporti. Può essere NULL.|  
|**mirror**|**tinyint**|Numero di mirroring (0-3).|  
  
## <a name="remarks"></a>Osservazioni  
 RESTOre VERIFYONLY FROM *backup_device* with LOADHISTORY popola le colonne della tabella **BackupMediaSet** con i valori appropriati dell'intestazione del set di supporti.  
  
 Per ridurre il numero di righe in questa tabella e in altre tabelle di backup e di cronologia, eseguire la [sp_delete_backuphistory](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md) stored procedure.  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di backup e ripristino &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [backupfile &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfile-transact-sql.md)   
 [backupfilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)   
 [backupmediaset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediaset-transact-sql.md)   
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [Tabelle di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
