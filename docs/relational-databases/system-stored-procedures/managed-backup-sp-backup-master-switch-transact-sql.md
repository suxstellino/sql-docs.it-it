---
description: managed_backup managed_backup.sp_backup_master_switch (Transact-SQL)
title: managed_backup managed_backup.sp_backup_master_switch (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_ backup_master_switch
- smart_admin.sp_backup_master_switch
- sp_ backup_master_switch_TSQL
- smart_admin.sp_backup_master_switch_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_ backup_master_switch
- smart_admin.sp_backup_master_switch
ms.assetid: 1ed2b2b2-c897-41cc-bed5-1c6bc47b9dd2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 97202d989a21002d7f24c886378fd3f6c6b35ebd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205307"
---
# <a name="managed_backupsp_backup_master_switch-transact-sql"></a>managed_backup managed_backup.sp_backup_master_switch (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Sospende o riprende il [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].  
  
 Utilizzare questa stored procedure per sospendere temporaneamente e riprendere il [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Ciò assicura che tutte le impostazioni di configurazione vengano conservate e mantenute quando riprendono le operazioni. Quando il [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] viene sospeso, il periodo di conservazione non viene applicato. Ciò significa che non sono presenti controlli per determinare se i file devono essere eliminati dall'archiviazione o se sono presenti file di backup danneggiati o un'interruzione nella catena di log.  
  

  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
EXEC managed_backup.sp_backup_master_switch   
                     [@new_state = ] { 0 | 1}  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Argomenti  
 @state  
 Imposta lo stato del [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Il @state parametro è di **bit**. Quando è impostato su un valore pari a 0, le operazioni vengono sospese e quando è impostato su un valore pari a 1, le operazioni riprendono.  
  
## <a name="return-code-value"></a>Valore del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="security"></a>Sicurezza  
 Vengono descritti i problemi di sicurezza relativi all'istruzione. Includere le autorizzazioni come sottosezione (titolo H3). Provare a includere altre sottosezioni per il concatenamento di proprietà e il controllo, se appropriate.  
  
### <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **db_backupoperator** database, con autorizzazioni **ALTER ANY CREDENTIAL** e autorizzazioni **Execute** per **sp_delete_backuphistory** stored procedure.  
  
## <a name="examples"></a>Esempio  
 L'esempio seguente può essere utilizzato per sospendere il [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] nell'istanza in cui viene eseguito:  
  
```  
Use msdb;  
GO  
EXEC managed_backup.sp_backup_master_switch @new_state=0;  
Go  
  
```  
  
 L'esempio seguente può essere utilizzato per riprendere il [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].  
  
```  
Use msdb;  
GO  
EXEC managed_backup.sp_backup_master_switch @new_state=1;  
Go  
  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Backup gestito di SQL Server in Microsoft Azure](../../relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure.md)  
  
  
