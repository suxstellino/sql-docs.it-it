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
ms.openlocfilehash: 6938d04f853f61fbb07a9cde443e286ba6bc5e98
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491231"
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
`[ @primary_server = ] 'primary_server'` Nome dell'istanza primaria [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] dell'oggetto nella log shipping configurazione. *primary_server* è **di tipo sysname** e non può essere NULL.  
  
`[ @primary_database = ] 'primary_database'` Nome del database nel server primario. *primary_database* è **sysname**, senza alcun valore predefinito.  
  
`[ @backup_source_directory = ] 'backup_source_directory'` Directory in cui vengono archiviati i file di backup del log delle transazioni dal server primario. *backup_source_directory* è **di tipo nvarchar(500)** e non può essere NULL.  
  
`[ @backup_destination_directory = ] 'backup_destination_directory'` Directory nel server secondario in cui vengono copiati i file di backup. *backup_destination_directory* è **di tipo nvarchar(500)** e non può essere NULL.  
  
`[ @file_retention_period = ] 'file_retention_period'` Periodo di tempo in minuti in cui verranno conservati i file di backup. *file_retention_period* è **di tipo int**, con valore predefinito NULL. Se non si specifica un valore, verrà utilizzato il valore 14420.  
  
`[ @monitor_server_security_mode = ] 'monitor_server_security_mode'` Modalità di sicurezza utilizzata per connettersi al server di monitoraggio.  
  
 1 = Autenticazione di Windows  
  
 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Autenticazione. *monitor_server_security_mode* è **di tipo bit** e non può essere NULL.  
  
`[ @monitor_server_login = ] 'monitor_server_login'` Nome utente dell'account utilizzato per accedere al server di monitoraggio.  
  
`[ @monitor_server_password = ] 'monitor_server_password'` Password dell'account utilizzato per accedere al server di monitoraggio.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 **sp_change_log_shipping_secondary_primary** deve essere eseguito dal database **master** nel server secondario. Questa stored procedure esegue le operazioni seguenti:  
  
1.  Modifica le impostazioni nei **record log_shipping_secondary** in base alle esigenze.  
  
2.  Se il server di monitoraggio è diverso dal  server secondario, modifica il record di monitoraggio nel log_shipping_monitor_secondary nel server di monitoraggio usando gli argomenti specificati, se necessario.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire questa procedura.  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni sul log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
