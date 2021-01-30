---
description: sp_add_log_shipping_primary_database (Transact-SQL)
title: sp_add_log_shipping_primary_database (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_log_shipping_primary_database
- sp_add_log_shipping_primary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_log_shipping_primary_database
ms.assetid: 69531611-113f-46b5-81a6-7bf496d0353c
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4c967127366c935b42ac33158982c6734114b2d8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206714"
---
# <a name="sp_add_log_shipping_primary_database-transact-sql"></a>sp_add_log_shipping_primary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Imposta il database primario per una configurazione di log shipping, inclusi il processo di backup, il record di monitoraggio locale e il record di monitoraggio remoto.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_add_log_shipping_primary_database [ @database = ] 'database',   
[ @backup_directory = ] 'backup_directory',   
[ @backup_share = ] 'backup_share',   
[ @backup_job_name = ] 'backup_job_name',   
[, [ @backup_retention_period = ] backup_retention_period]  
[, [ @monitor_server = ] 'monitor_server']  
[, [ @monitor_server_security_mode = ] monitor_server_security_mode]  
[, [ @monitor_server_login = ] 'monitor_server_login']  
[, [ @monitor_server_password = ] 'monitor_server_password']  
[, [ @backup_threshold = ] backup_threshold ]   
[, [ @threshold_alert = ] threshold_alert ]   
[, [ @threshold_alert_enabled = ] threshold_alert_enabled ]   
[, [ @history_retention_period = ] history_retention_period ]  
[, [ @backup_job_id = ] backup_job_id OUTPUT ]  
[, [ @primary_id = ] primary_id OUTPUT]  
[, [ @backup_compression = ] backup_compression_option ]  
  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @database = ] 'database'` Nome del database primario log shipping. il *database* è di **tipo sysname** e non prevede alcun valore predefinito e non può essere null.  
  
`[ @backup_directory = ] 'backup_directory'` È il percorso della cartella di backup nel server primario. *backup_directory* è di **tipo nvarchar (500)** e non prevede alcun valore predefinito e non può essere null.  
  
`[ @backup_share = ] 'backup_share'` È il percorso di rete della directory di backup nel server primario. *backup_share* è di **tipo nvarchar (500)** e non prevede alcun valore predefinito e non può essere null.  
  
`[ @backup_job_name = ] 'backup_job_name'` Nome del processo di SQL Server Agent nel server primario che copia il backup nella cartella di backup. *backup_job_name* è di **tipo sysname** e non può essere null.  
  
`[ @backup_retention_period = ] backup_retention_period` Periodo di tempo, in minuti, per il quale il file di backup del log viene mantenuto nella directory di backup del server primario. *backup_retention_period* è di **tipo int** e non prevede alcun valore predefinito e non può essere null.  
  
`[ @monitor_server = ] 'monitor_server'` Nome del server di monitoraggio. *Monitor_server* è di **tipo sysname** e non prevede alcun valore predefinito e non può essere null.  
  
`[ @monitor_server_security_mode = ] monitor_server_security_mode` Modalità di sicurezza utilizzata per la connessione al server di monitoraggio.  
  
 1 = Autenticazione di Windows.  
  
 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticazione di. *monitor_server_security_mode* è di **bit** e non può essere null.  
  
`[ @monitor_server_login = ] 'monitor_server_login'` Nome utente dell'account utilizzato per accedere al server di monitoraggio.  
  
`[ @monitor_server_password = ] 'monitor_server_password'` Password dell'account utilizzato per accedere al server di monitoraggio.  
  
`[ @backup_threshold = ] backup_threshold` Periodo di tempo, in minuti, dopo l'ultimo backup prima che venga generato un errore di *threshold_alert* . *backup_threshold* è di **tipo int** e il valore predefinito è 60 minuti.  
  
`[ @threshold_alert = ] threshold_alert` Avviso da generare quando viene superata la soglia di backup. *threshold_alert* è di **tipo int** e il valore predefinito è 14.420.  
  
`[ @threshold_alert_enabled = ] threshold_alert_enabled` Specifica se verrà generato un avviso quando viene superato *backup_threshold* . Il valore predefinito 0 indica che l'avviso è disabilitato e non verrà generato. *threshold_alert_enabled* è di **bit**.  
  
`[ @history_retention_period = ] history_retention_period` Periodo di tempo in minuti in cui la cronologia verrà mantenuta. *history_retention_period* è di **tipo int** e il valore predefinito è null. Se non si specifica un valore, verrà utilizzato il valore 14420.  
  
`[ @backup_job_id = ] backup_job_id OUTPUT`[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ID del processo di Agent associato al processo di backup nel server primario. *backup_job_id* è di tipo **uniqueidentifier** e non può essere null.  
  
`[ @primary_id = ] primary_id OUTPUT` ID del database primario per la configurazione del log shipping. *primary_id* è di tipo **uniqueidentifier** e non può essere null.  
  
`[ @backup_compression = ] backup_compression_option` Specifica se una configurazione log shipping utilizza la [compressione dei backup](../../relational-databases/backup-restore/backup-compression-sql-server.md). Questo parametro è supportato solo in [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] o versione successiva.  
  
 0 = disabilitati. I backup del log non vengono mai compressi.  
  
 1 = abilitati. I backup del log vengono sempre compressi.  
  
 2 = utilizzare l'impostazione dell' [opzione di configurazione del server visualizzazione o configurazione del server backup compression default](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md). Si tratta del valore predefinito.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 **sp_add_log_shipping_primary_database** deve essere eseguito dal database **Master** nel server primario. Questa stored procedure esegue le funzioni seguenti:  
  
1.  Genera un ID primario e aggiunge una voce per il database primario nella tabella **log_shipping_primary_databases** usando gli argomenti forniti.  
  
2.  Crea un processo di backup per il database primario disabilitato.  
  
3.  Imposta l'ID del processo di backup nella voce **log_shipping_primary_databases** sull'ID processo del processo di backup.  
  
4.  Aggiunge un record di monitoraggio locale nella tabella **log_shipping_monitor_primary** sul server primario utilizzando gli argomenti specificati.  
  
5.  Se il server di monitoraggio è diverso dal server primario, aggiunge un record di monitoraggio in **log_shipping_monitor_primary** sul server di monitoraggio utilizzando gli argomenti specificati.  
  
## <a name="permissions"></a>Autorizzazioni  
 Questa procedura può essere eseguita solo dai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 In questo esempio il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] viene aggiunto come database primario in una configurazione per il log shipping.  
  
```  
DECLARE @LS_BackupJobId AS uniqueidentifier ;  
DECLARE @LS_PrimaryId AS uniqueidentifier ;  
  
EXEC master.dbo.sp_add_log_shipping_primary_database   
@database = N'AdventureWorks'   
,@backup_directory = N'c:\lsbackup'   
,@backup_share = N'\\tribeca\lsbackup'   
,@backup_job_name = N'LSBackup_AdventureWorks'   
,@backup_retention_period = 1440  
,@monitor_server = N'rockaway'   
,@monitor_server_security_mode = 1   
,@backup_threshold = 60   
,@threshold_alert = 0   
,@threshold_alert_enabled = 0   
,@history_retention_period = 1440   
,@backup_job_id = @LS_BackupJobId OUTPUT   
,@primary_id = @LS_PrimaryId OUTPUT   
,@overwrite = 1   
,@backup_compression = 0;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni sul log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
