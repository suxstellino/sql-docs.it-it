---
description: sp_add_log_shipping_secondary_database (Transact-SQL)
title: sp_add_log_shipping_secondary_database (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_log_shipping_secondary_database
- sp_add_log_shipping_secondary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_log_shipping_secondary_database
ms.assetid: d29e1c24-3a3c-47a4-a726-4584afa6038a
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d13a58428c4fefe2cc19668de22e604f721d0a96
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206712"
---
# <a name="sp_add_log_shipping_secondary_database-transact-sql"></a>sp_add_log_shipping_secondary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Imposta un database secondario per il log shipping.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_add_log_shipping_secondary_database  
[ @secondary_database = ] 'secondary_database',  
[ @primary_server = ] 'primary_server',   
[ @primary_database = ] 'primary_database',  
[, [ @restore_delay = ] 'restore_delay']  
[, [ @restore_all = ] 'restore_all']  
[, [ @restore_mode = ] 'restore_mode']  
[, [ @disconnect_users = ] 'disconnect_users']  
[, [ @block_size = ] 'block_size']  
[, [ @buffer_count = ] 'buffer_count']  
[, [ @max_transfer_size = ] 'max_transfer_size']  
[, [ @restore_threshold = ] 'restore_threshold']   
[, [ @threshold_alert = ] 'threshold_alert']   
[, [ @threshold_alert_enabled = ] 'threshold_alert_enabled']   
[, [ @history_retention_period = ] 'history_retention_period']  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @secondary_database = ] 'secondary_database'` Nome del database secondario. *secondary_database* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @primary_server = ] 'primary_server'` Nome dell'istanza primaria di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] nella configurazione log shipping. *primary_server* è di **tipo sysname** e non può essere null.  
  
`[ @primary_database = ] 'primary_database'` Nome del database nel server primario. *primary_database* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @restore_delay = ] 'restore_delay'` Quantità di tempo, in minuti, che il server secondario attende prima di ripristinare un file di backup specificato. *restore_delay* è di **tipo int** e non può essere null. Il valore predefinito è 0.  
  
`[ @restore_all = ] 'restore_all'` Se impostato su 1, il server secondario ripristina tutti i backup del log delle transazioni disponibili al momento dell'esecuzione del processo di ripristino. In caso contrario, l'operazione viene arrestata dopo il ripristino di un file. *restore_all* è di **bit** e non può essere null.  
  
`[ @restore_mode = ] 'restore_mode'` Modalità di ripristino per il database secondario.  
  
 0 = Ripristina log con NORECOVERY.  
  
 1 = Ripristina log con STANDBY.  
  
 il *ripristino* è di **bit** e non può essere null.  
  
`[ @disconnect_users = ] 'disconnect_users'` Se impostato su 1, gli utenti vengono disconnessi dal database secondario quando viene eseguita un'operazione di ripristino. Valore predefinito = 0. la *disconnessione* degli utenti è di **bit** e non può essere null.  
  
`[ @block_size = ] 'block_size'` Dimensione, in byte, utilizzata come dimensione del blocco per il dispositivo di backup. *block_size* è di **tipo int** e il valore predefinito è-1.  
  
`[ @buffer_count = ] 'buffer_count'` Numero totale di buffer utilizzati dall'operazione di backup o ripristino. *buffer_count* è di **tipo int** e il valore predefinito è-1.  
  
`[ @max_transfer_size = ] 'max_transfer_size'` Dimensione, in byte, della richiesta di input o output massima rilasciata da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al dispositivo di backup. *max_transfersize* è di **tipo int** e può essere null.  
  
`[ @restore_threshold = ] 'restore_threshold'` Numero di minuti che possono trascorrere tra le operazioni di ripristino prima che venga generato un avviso. *restore_threshold* è di **tipo int** e non può essere null.  
  
`[ @threshold_alert = ] 'threshold_alert'` Avviso da generare quando viene superata la soglia di backup. *threshold_alert* è di **tipo int** e il valore predefinito è 14.420.  
  
`[ @threshold_alert_enabled = ] 'threshold_alert_enabled'` Specifica se viene generato un avviso quando viene superato *backup_threshold* . Il valore 1 (valore predefinito) indica che l'avviso viene generato. *threshold_alert_enabled* è di **bit**.  
  
`[ @history_retention_period = ] 'history_retention_period'` Periodo di tempo in minuti in cui viene mantenuta la cronologia. *history_retention_period* è di **tipo int** e il valore predefinito è null. Se non si specifica un valore, verrà utilizzato il valore 14420.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 **sp_add_log_shipping_secondary_database** deve essere eseguito dal database **Master** nel server secondario. Questa stored procedure esegue le operazioni seguenti:  
  
1.  prima di questo stored procedure è necessario chiamare **sp_add_log_shipping_secondary_primary** per inizializzare le informazioni del database di log shipping primario sul server secondario.  
  
2.  Aggiunge una voce per il database secondario in **log_shipping_secondary_databases** usando gli argomenti forniti.  
  
3.  Aggiunge un record di monitoraggio locale in **log_shipping_monitor_secondary** sul server secondario utilizzando gli argomenti specificati.  
  
4.  Se il server di monitoraggio è diverso dal server secondario, aggiunge un record di monitoraggio in **log_shipping_monitor_secondary** sul server di monitoraggio utilizzando gli argomenti specificati.  
  
## <a name="permissions"></a>Autorizzazioni  
 Questa procedura può essere eseguita solo dai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 Questo esempio illustra l'uso del stored procedure **sp_add_log_shipping_secondary_database** per aggiungere il database **LogShipAdventureWorks** come database secondario in una configurazione log shipping con il database primario che [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] risiede nel server primario Tribeca.  
  
```  
EXEC master.dbo.sp_add_log_shipping_secondary_database   
@secondary_database = N'LogShipAdventureWorks'   
,@primary_server = N'TRIBECA'   
,@primary_database = N'AdventureWorks2012'   
,@restore_delay = 0   
,@restore_mode = 1   
,@disconnect_users = 0   
,@restore_threshold = 45     
,@threshold_alert_enabled = 0   
,@history_retention_period = 1440 ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni sul log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
