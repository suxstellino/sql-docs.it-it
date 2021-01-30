---
description: sp_dbmmonitorupdate (Transact-SQL)
title: sp_dbmmonitorupdate (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dbmmonitorupdate
- sp_dbmmonitorupdate_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dbmmonitorupdate
- database mirroring [SQL Server], monitoring
ms.assetid: 9ceb9611-4929-44ee-a406-c39ba2720fd5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7a18e87dd51da9869afa2b180a5fd55ef62c304e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203140"
---
# <a name="sp_dbmmonitorupdate-transact-sql"></a>sp_dbmmonitorupdate (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Aggiorna la tabella di stato di Monitoraggio mirroring del database inserendo una nuova riga di tabella per ogni database con mirroring e tronca le righe precedenti al periodo di memorizzazione corrente. Il periodo di memorizzazione predefinito è 7 giorni (168 ore). Quando si aggiorna la tabella, **sp_dbmmonitorupdate** valuta le metriche delle prestazioni.  
  
> [!NOTE]  
>  Alla prima esecuzione di **sp_dbmmonitorupdate** , vengono creati la tabella di stato di mirroring del database e il ruolo predefinito del database **dbm_monitor** nel database **msdb** .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_dbmmonitorupdate [ database_name ]  
```  
  
## <a name="arguments"></a>Argomenti  
 *database_name*  
 Nome del database per cui aggiornare lo stato di mirroring. Se *database_name* non viene specificato, la stored procedure aggiorna la tabella di stato per ogni database con mirroring nell'istanza del server.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Nessuno  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 **sp_dbmmonitorupdate** possono essere eseguite solo nel contesto del database **msdb** .  
  
 Se una colonna della tabella di stato non è valida per il ruolo di un partner, il valore è NULL per tale partner. Una colonna include inoltre valori NULL se le informazioni rilevanti non sono disponibili, ad esempio durante il failover o il riavvio del server.  
  
 Dopo che **sp_dbmmonitorupdate** crea il ruolo predefinito del database **dbm_monitor** nel database **msdb** , i membri del ruolo predefinito del server **sysadmin** possono aggiungere qualsiasi utente al ruolo predefinito del database **dbm_monitor** . Il ruolo **dbm_monitor** consente ai membri di visualizzare lo stato di mirroring del database, ma non di aggiornarlo, ma non di visualizzare né configurare gli eventi di mirroring del database.  
  
 Quando si aggiorna lo stato di mirroring di un database, **sp_dbmmonitorupdate** controlla il valore più recente della metrica delle prestazioni di mirroring per la quale è stata specificata una soglia di avviso. Se il valore supera la soglia, la procedura aggiunge un evento informativo al log eventi. Tutti valori sono medie eseguite dopo l'ultimo aggiornamento. Per altre informazioni, vedere [Usare valori di soglia avvisi e avvisi sulle metriche delle prestazioni di mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene aggiornato lo stato di mirroring solo per il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
USE msdb;  
EXEC sp_dbmmonitorupdate AdventureWorks2012 ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Monitoraggio del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [sp_dbmmonitorchangealert &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangealert-transact-sql.md)   
 [sp_dbmmonitorchangemonitoring &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md)   
 [sp_dbmmonitordropalert &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbmmonitordropalert-transact-sql.md)   
 [sp_dbmmonitorhelpalert &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpalert-transact-sql.md)   
 [sp_dbmmonitorhelpmonitoring &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpmonitoring-transact-sql.md)   
 [sp_dbmmonitorresults &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md)  
  
  
