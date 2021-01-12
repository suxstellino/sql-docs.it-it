---
title: BackupMediaSet (Transact-SQL) | Microsoft Docs
description: Riferimento per BackupMediaSet, che contiene una riga per ogni set di supporti di backup.
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- backupmediaset
- backupmediaset_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- backup media [SQL Server], backupmediaset system table
- backupmediaset system table
author: cawrites
ms.author: chadam
ms.openlocfilehash: 10b4d744e7ef4e0d11a9788580ea7f8c5a67bd1f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091581"
---
# <a name="backupmediaset-transact-sql"></a>backupmediaset (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Contiene una riga per ogni set di supporti di backup. Questa tabella è archiviata nel database **msdb** .  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**media_set_id**|**int**|Numero di identificazione univoco del set di supporti. Identità, chiave primaria.|  
|**media_uuid**|**uniqueidentifier**|UUID del set di supporti. Tutti i [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] set di supporti hanno un UUID.<br /><br /> Per le versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , tuttavia, se un set di supporti contiene un solo gruppo di supporti, la colonna **media_uuid** potrebbe essere null (**media_family_count** è 1).|  
|**media_family_count**|**tinyint**|Numero di gruppi di supporti nel set di supporti. Può essere NULL.|  
|**nome**|**nvarchar(128)**|Nome del set di supporti. Può essere NULL.<br /><br /> Per ulteriori informazioni, vedere MEDIAname e MEDIADESCRIPTION in [BACKUP &#40;&#41;Transact-SQL ](../../t-sql/statements/backup-transact-sql.md).|  
|**description**|**nvarchar(255)**|Descrizione in formato testo del set di supporti. Può essere NULL.<br /><br /> Per ulteriori informazioni, vedere MEDIAname e MEDIADESCRIPTION in [BACKUP &#40;&#41;Transact-SQL ](../../t-sql/statements/backup-transact-sql.md).|  
|**software_name**|**nvarchar(128)**|Nome del software di backup con cui è stata scritta l'etichetta del supporto. Può essere NULL.|  
|**software_vendor_id**|**int**|Numero di identificazione del produttore del software con cui è stata scritta l'etichetta del supporto di backup. Può essere NULL.<br /><br /> Il valore per [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è esadecimale 0x1200.|  
|**MTF_major_version**|**tinyint**|Numero principale della versione di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Tape Format utilizzata per generare il set di supporti. Può essere NULL.|  
|**mirror_count**|**tinyint**|Numero di mirroring nel set di supporti.|  
|**is_password_protected**|**bit**|Set di supporti protetto da password:<br /><br /> 0 = non protetto<br /><br /> 1 = protetto|  
|**is_compressed**|**bit**|Specifica se il backup è compresso:<br /><br /> 0 = non compresso<br /><br /> 1 = compresso<br /><br /> Durante un aggiornamento di **msdb** , questo valore è impostato su null. che indica un backup non compresso.|  
|**is_encrypted**|**bit**|Specifica se il backup è crittografato:<br /><br /> 0 = Non crittografato<br /><br /> 1 = Crittografato|  
  
## <a name="remarks"></a>Osservazioni  
 RESTOre VERIFYONLY FROM *backup_device* with LOADHISTORY popola le colonne della tabella **BackupMediaSet** con i valori appropriati dell'intestazione del set di supporti.  
  
 Per ridurre il numero di righe in questa tabella e in altre tabelle di backup e di cronologia, eseguire la [sp_delete_backuphistory](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md) stored procedure.  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di backup e ripristino &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [backupfile &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfile-transact-sql.md)   
 [backupfilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)   
 [backupmediafamily &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)   
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [Tabelle di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
