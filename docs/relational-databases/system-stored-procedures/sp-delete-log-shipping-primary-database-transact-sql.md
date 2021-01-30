---
description: sp_delete_log_shipping_primary_database (Transact-SQL)
title: sp_delete_log_shipping_primary_database (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_log_shipping_primary_database
- sp_delete_log_shipping_primary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_log_shipping_primary_database
ms.assetid: cb1d5d00-2805-4d47-bd04-545232067345
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a42bb060044c35e4d2569e23e24eb8ba9bc46fd7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211136"
---
# <a name="sp_delete_log_shipping_primary_database-transact-sql"></a>sp_delete_log_shipping_primary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Questa stored procedure rimuove il log shipping del database primario, compreso il processo di backup, e la cronologia locale e remota. Utilizzare questo stored procedure solo dopo aver rimosso i database secondari utilizzando **sp_delete_log_shipping_primary_secondary**.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_delete_log_shipping_primary_database  
[ @database = ] 'database'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @database = ] 'database'` Nome del database primario log shipping. il *database* è di **tipo sysname** e non prevede alcun valore predefinito e non può essere null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 No.  
  
## <a name="remarks"></a>Osservazioni  
 **sp_delete_log_shipping_primary_database** deve essere eseguito dal database **Master** nel server primario. Questa stored procedure esegue le operazioni seguenti:  
  
1.  Elimina il processo di backup per il database primario specificato.  
  
2.  Rimuove il record di monitoraggio locale in **log_shipping_monitor_primary** sul server primario.  
  
3.  Rimuove le voci corrispondenti in **log_shipping_monitor_history_detail** e **log_shipping_monitor_error_detail**.  
  
4.  Se il server di monitoraggio è diverso dal server primario, rimuove il record di monitoraggio in **log_shipping_monitor_primary** sul server di monitoraggio.  
  
5.  Rimuove le voci corrispondenti in **log_shipping_monitor_history_detail** e **log_shipping_monitor_error_detail** sul server di monitoraggio.  
  
6.  Rimuove la voce **log_shipping_primary_databases** per il database primario.  
  
7.  Chiama **sp_delete_log_shipping_alert_job** sul server di monitoraggio.  

## <a name="permissions"></a>Autorizzazioni  
 Questa procedura può essere eseguita solo dai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 Questo esempio illustra l'uso di **sp_delete_log_shipping_primary_database** per eliminare il database primario **AdventureWorks2012**.  
  
```  
EXEC master.dbo.sp_delete_log_shipping_primary_database @database = N'AdventureWorks2012';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni sul log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
