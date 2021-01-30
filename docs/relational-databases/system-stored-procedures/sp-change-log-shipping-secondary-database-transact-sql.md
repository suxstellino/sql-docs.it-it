---
description: sp_change_log_shipping_secondary_database (Transact-SQL)
title: sp_change_log_shipping_secondary_database (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_change_log_shipping_secondary_database
- sp_change_log_shipping_secondary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_change_log_shipping_secondary_database
ms.assetid: 3ebcf2f1-980f-4543-a84b-fbaeea54eeac
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 50383a1389c6327ae9caa6dcabf69bdcb8c720b4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209357"
---
# <a name="sp_change_log_shipping_secondary_database-transact-sql"></a>sp_change_log_shipping_secondary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica le impostazioni del database secondario.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_change_log_shipping_secondary_database  
[ @secondary_database = ] 'secondary_database',  
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
`[ @restore_delay = ] 'restore_delay'` Quantità di tempo, in minuti, che il server secondario attende prima di ripristinare un file di backup specificato. *restore_delay* è di **tipo int** e non può essere null. Il valore predefinito è 0.  
  
`[ @restore_all = ] 'restore_all'` Se impostato su 1, il server secondario ripristina tutti i backup del log delle transazioni disponibili al momento dell'esecuzione del processo di ripristino. In caso contrario, l'operazione viene arrestata dopo il ripristino di un file. *restore_all* è di **bit** e non può essere null.  
  
`[ @restore_mode = ] 'restore_mode'` Modalità di ripristino per il database secondario.  
  
 0 = ripristino log con NORECOVERY.  
  
 1 = Ripristina log con STANDBY.  
  
 il *ripristino* è di **bit** e non può essere null.  
  
`[ @disconnect_users = ] 'disconnect_users'` Se impostato su 1, gli utenti vengono disconnessi dal database secondario quando viene eseguita un'operazione di ripristino. Valore predefinito = 0. *disconnect_users* è di **bit** e non può essere null.  
  
`[ @block_size = ] 'block_size'` Dimensione, in byte, utilizzata come dimensione del blocco per il dispositivo di backup. *block_size* è di **tipo int** e il valore predefinito è-1.  
  
`[ @buffer_count = ] 'buffer_count'` Numero totale di buffer utilizzati dall'operazione di backup o ripristino. *buffer_count* è di **tipo int** e il valore predefinito è-1.  
  
`[ @max_transfer_size = ] 'max_transfer_size'` Dimensione, in byte, della richiesta di input o output massima rilasciata da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al dispositivo di backup. *max_transfersize* è di **tipo int** e può essere null.  
  
`[ @restore_threshold = ] 'restore_threshold'` Numero di minuti che possono trascorrere tra le operazioni di ripristino prima che venga generato un avviso. *restore_threshold* è di **tipo int** e non può essere null.  
  
`[ @threshold_alert = ] 'threshold_alert'` Avviso da generare quando viene superata la soglia di ripristino. *threshold_alert* è di **tipo int** e il valore predefinito è 14421.  
  
`[ @threshold_alert_enabled = ] 'threshold_alert_enabled'` Specifica se verrà generato un avviso quando viene superato *restore_threshold*. 1 = abilitato; 0 = disabilitato. *threshold_alert_enabled* è di **bit** e non può essere null.  
  
`[ @history_retention_period = ] 'history_retention_period'` Periodo di tempo in minuti in cui la cronologia verrà mantenuta. *history_retention_period* è di **tipo int**. Se non è specificato alcun valore, verrà utilizzato il valore 1440.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 **sp_change_log_shipping_secondary_database** deve essere eseguito dal database **Master** nel server secondario. Questa stored procedure esegue le operazioni seguenti:  
  
1.  Modifica le impostazioni nei record di **log_shipping_secondary_database** , se necessario.  
  
2.  Modifica il record di monitoraggio locale in **log_shipping_monitor_secondary** sul server secondario utilizzando gli argomenti specificati, se necessario.  

## <a name="permissions"></a>Autorizzazioni  
 Questa procedura può essere eseguita solo dai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 In questo esempio viene illustrato l'utilizzo di **sp_change_log_shipping_secondary_database** per aggiornare i parametri del database secondario per il database **LogShipAdventureWorks**.  
  
```  
EXEC master.dbo.sp_change_log_shipping_secondary_database   
 @secondary_database =  'LogShipAdventureWorks'  
,  @restore_delay = 0  
,  @restore_all = 1  
,  @restore_mode = 0  
,  @disconnect_users = 0  
,  @threshold_alert = 14420  
,  @threshold_alert_enabled = 1  
,  @history_retention_period = 14420;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni sul log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
