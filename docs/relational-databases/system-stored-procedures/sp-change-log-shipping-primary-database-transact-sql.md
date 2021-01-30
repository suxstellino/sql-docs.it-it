---
description: sp_change_log_shipping_primary_database (Transact-SQL)
title: sp_change_log_shipping_primary_database (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_change_log_shipping_primary_database
- sp_change_log_shipping_primary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_change_log_shipping_primary_database
ms.assetid: 8c9dce6b-d2a3-4ca7-a832-8f59a5adb214
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e1bcd150ff810f5a4a16deb09a7ea72956ccd681
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181353"
---
# <a name="sp_change_log_shipping_primary_database-transact-sql"></a>sp_change_log_shipping_primary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica le impostazioni del database primario.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_change_log_shipping_primary_database [ @database = ] 'database'  
[, [ @backup_directory = ] 'backup_directory']   
[, [ @backup_share = ] 'backup_share']   
[, [ @backup_retention_period = ] 'backup_retention_period']  
[, [ @monitor_server_security_mode = ] 'monitor_server_security_mode']  
[, [ @monitor_server_login = ] 'monitor_server_login']  
[, [ @monitor_server_password = ] 'monitor_server_password']  
[, [ @backup_threshold = ] 'backup_threshold']   
[, [ @threshold_alert = ] 'threshold_alert']   
[, [ @threshold_alert_enabled = ] 'threshold_alert_enabled']   
[, [ @history_retention_period = ] 'history_retention_period']  
[, [ @backup_compression = ] backup_compression_option ]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @database = ] 'database'` Nome del database nel server primario. *primary_database* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @backup_directory = ] 'backup_directory'` È il percorso della cartella di backup nel server primario. *backup_directory* è di **tipo nvarchar (500)** e non prevede alcun valore predefinito e non può essere null.  
  
`[ @backup_share = ] 'backup_share'` È il percorso di rete della directory di backup nel server primario. *backup_share* è di **tipo nvarchar (500)** e non prevede alcun valore predefinito e non può essere null.  
  
`[ @backup_retention_period = ] 'backup_retention_period'` Periodo di tempo, in minuti, per il quale il file di backup del log viene mantenuto nella directory di backup del server primario. *backup_retention_period* è di **tipo int** e non prevede alcun valore predefinito e non può essere null.  
  
`[ @monitor_server_security_mode = ] 'monitor_server_security_mode'` Modalità di sicurezza utilizzata per la connessione al server di monitoraggio.  
  
 1 = Autenticazione di Windows.  
  
 0 = Autenticazione di SQL Server.  
  
 *monitor_server_security_mode* è di **bit** e non può essere null.  
  
`[ @monitor_server_login = ] 'monitor_server_login'` Nome utente dell'account utilizzato per accedere al server di monitoraggio.  
  
`[ @monitor_server_password = ] 'monitor_server_password'` Password dell'account utilizzato per accedere al server di monitoraggio.  
  
`[ @backup_threshold = ] 'backup_threshold'` Periodo di tempo, in minuti, dopo l'ultimo backup prima che venga generato un errore di *threshold_alert* . *backup_threshold* è di **tipo int** e il valore predefinito è 60 minuti.  
  
`[ @threshold_alert = ] 'threshold_alert'` Avviso da generare quando viene superata la soglia di backup. *threshold_alert* è di **tipo int** e non può essere null.  
  
`[ @threshold_alert_enabled = ] 'threshold_alert_enabled'` Specifica se viene generato un avviso quando viene superato *backup_threshold* .  
  
 1 = Abilitato.  
  
 0 = Disabilitato.  
  
 *threshold_alert_enabled* è di **bit** e non può essere null.  
  
`[ @history_retention_period = ] 'history_retention_period'` Periodo di tempo in minuti in cui viene mantenuta la cronologia. *history_retention_period* è di **tipo int**. Se non è specificato alcun valore, viene utilizzato il valore 14420.  
  
`[ @backup_compression = ] backup_compression_option` Specifica se una configurazione log shipping utilizza la [compressione dei backup](../../relational-databases/backup-restore/backup-compression-sql-server.md). Questo parametro è supportato solo in [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] o versione successiva.  
  
 0 = disabilitati. I backup del log non vengono mai compressi.  
  
 1 = abilitati. I backup del log vengono sempre compressi.  
  
 2 = utilizzare l'impostazione dell' [opzione di configurazione del server visualizzazione o configurazione del server backup compression default](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md). Si tratta del valore predefinito.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 **sp_change_log_shipping_primary_database** deve essere eseguito dal database **Master** nel server primario. Questa stored procedure esegue le operazioni seguenti:  
  
1.  Modifica le impostazioni nel record **log_shipping_primary_database** , se necessario.  
  
2.  Modifica il record locale in **log_shipping_monitor_primary** sul server primario utilizzando gli argomenti specificati, se necessario.  
  
3.  Se il server di monitoraggio è diverso dal server primario, le modifiche apportate **log_shipping_monitor_primary** nel server di monitoraggio utilizzando gli argomenti specificati, se necessario.  
  
## <a name="permissions"></a>Autorizzazioni  
 Questa procedura può essere eseguita solo dai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 Questo esempio illustra l'uso di **sp_change_log_shipping_primary_database** per aggiornare le impostazioni associate al database primario [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
```  
EXEC master.dbo.sp_change_log_shipping_primary_database   
 @database = N'AdventureWorks'   
, @backup_directory = N'c:\LogShipping'   
, @backup_share = N'\\tribeca\LogShipping'   
, @backup_retention_period = 1440   
, @backup_threshold = 60   
, @threshold_alert = 0   
, @threshold_alert_enabled = 1   
, @history_retention_period = 1440   
,@monitor_server_security_mode = 1  
,@backup_compression = 1;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni sul log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [log_shipping_primary_databases &#40;Transact-SQL&#41;](../../relational-databases/system-tables/log-shipping-primary-databases-transact-sql.md)  
  
  
