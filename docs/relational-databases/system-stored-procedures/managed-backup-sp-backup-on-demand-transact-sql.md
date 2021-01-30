---
description: managed_backup managed_backup.sp_backup_on_demand (Transact-SQL)
title: managed_backup managed_backup.sp_backup_on_demand (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- smart_admin.sp_backup_on_demand
- smart_admin.sp_backup_on_demand_TSQL
- sp_backup_on_demand_TSQL
- sp_backup_on_demand
dev_langs:
- TSQL
helpviewer_keywords:
- smart_admin.sp_backup_on_demand
- sp_backup_on_demand
ms.assetid: 638f809f-27fa-4c44-a549-9cf37ecc920c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 480338a6194b07f15d8c139a6675649e5c4f8e7d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205298"
---
# <a name="managed_backupsp_backup_on_demand-transact-sql"></a>managed_backup managed_backup.sp_backup_on_demand (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Richiede al [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] di eseguire un backup del database specificato.  
  
 Utilizzare questa stored procedure per l'esecuzione dei backup ad hoc per un database configurato con il [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. In questo modo si impedisce che le interruzioni della catena di backup e [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] dei processi siano compatibili e che il backup venga archiviato nello stesso contenitore di archiviazione BLOB di Azure.  
  
 Al completamento del backup, viene restituito il percorso del file di backup completo che include il nome e la posizione del nuovo file di backup risultante dall'operazione di backup.  
  
 Viene restituito un errore se il [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] è in fase di esecuzione di un backup di un determinato tipo per il database specificato. In questo caso, il messaggio di errore restituito include il percorso del file di backup completo in cui il backup corrente viene caricato.  
   
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
EXEC managed_backup.sp_backup_on_demand   
[@database_name  =]  'database name',[@type = ] { 'Database' | 'Log' }  
  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Argomenti  
 @database_name  
 Nome del database in cui deve essere eseguito il backup. @database_nameÈ di **tipo sysname**.  
  
 @type  
 Tipo di backup da eseguire: database o log. Il @type parametro è di **tipo nvarchar (32)**.  
  
## <a name="return-code-value"></a>Valore del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **db_backupoperator** database, con autorizzazioni **ALTER ANY CREDENTIAL** e autorizzazioni **Execute** per **sp_delete_backuphistory** stored procedure.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene eseguita una richiesta di backup del database per il database ' TestDB '. Per questo database è stato abilitato il [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].  
  
```  
Use MSDB  
Go  
EXEC managed_backup.sp_backup_on_demand  
 @database_name = 'TestDB'  
,@type = 'Database'  
  
```  
  
 Per ogni frammento di codice, selezionare 'tsql' nel campo dell'attributo di linguaggio.  
  
  
