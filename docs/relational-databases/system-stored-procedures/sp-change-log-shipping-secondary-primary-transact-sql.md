---
description: sp_change_log_shipping_secondary_primary (Transact-SQL)
title: sp_change_log_shipping_secondary_primary (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_change_log_shipping_secondary_primary
- sp_change_log_shipping_secondary_primary_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_change_log_shipping_secondary_primary
ms.assetid: 5bcb4df7-6df3-4f2b-9207-b97b5addf2a6
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 8352706512e71a5cba3a0b22befe2b7416d276bc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209351"
---
# <a name="sp_change_log_shipping_secondary_primary-transact-sql"></a>sp_change_log_shipping_secondary_primary (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica le impostazioni del database secondario.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_change_log_shipping_secondary_primary  
[ @primary_server = ] 'primary_server',  
[ @primary_database = ] 'primary_database',  
[, [ @backup_source_directory = ] 'backup_source_directory']  
[, [ @backup_destination_directory = ] 'backup_destination_directory']  
[, [ @file_retention_period = ] file_retention_period]  
[, [ @monitor_server_security_mode = ] monitor_server_security_mode]  
[, [ @monitor_server_login = ] 'monitor_server_login']  
[, [ @monitor_server_password = ] 'monitor_server_password']  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @primary_server = ] 'primary_server'` Nome dell'istanza primaria di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] nella configurazione log shipping. *primary_server* è di **tipo sysname** e non può essere null.  
  
`[ @primary_database = ] 'primary_database'` Nome del database nel server primario. *primary_database* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @backup_source_directory = ] 'backup_source_directory'` Directory in cui vengono archiviati i file di backup del log delle transazioni dal server primario. *backup_source_directory* è di **tipo nvarchar (500)** e non può essere null.  
  
`[ @backup_destination_directory = ] 'backup_destination_directory'` Directory nel server secondario in cui vengono copiati i file di backup. *backup_destination_directory* è di **tipo nvarchar (500)** e non può essere null.  
  
`[ @file_retention_period = ] 'file_retention_period'` Periodo di tempo in minuti in cui la cronologia verrà mantenuta. *history_retention_period* è di **tipo int** e il valore predefinito è null. Se non si specifica un valore, verrà utilizzato il valore 14420.  
  
`[ @monitor_server_security_mode = ] 'monitor_server_security_mode'` Modalità di sicurezza utilizzata per la connessione al server di monitoraggio.  
  
 1 = Autenticazione di Windows  
  
 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticazione di. *monitor_server_security_mode* è di **bit** e non può essere null.  
  
`[ @monitor_server_login = ] 'monitor_server_login'` Nome utente dell'account utilizzato per accedere al server di monitoraggio.  
  
`[ @monitor_server_password = ] 'monitor_server_password'` Password dell'account utilizzato per accedere al server di monitoraggio.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 **sp_change_log_shipping_secondary_primary** deve essere eseguito dal database **Master** nel server secondario. Questa stored procedure esegue le operazioni seguenti:  
  
1.  Modifica le impostazioni nei record di **log_shipping_secondary** , se necessario.  
  
2.  Se il server di monitoraggio è diverso dal server secondario, modifica il record di monitoraggio in **log_shipping_monitor_secondary** sul server di monitoraggio utilizzando gli argomenti specificati, se necessario.  
  
## <a name="permissions"></a>Autorizzazioni  
 Questa procedura può essere eseguita solo dai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni sul log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
