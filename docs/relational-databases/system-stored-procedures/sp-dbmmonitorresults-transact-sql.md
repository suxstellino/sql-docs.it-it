---
description: sp_dbmmonitorresults (Transact-SQL)
title: sp_dbmmonitorresults (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dbmmonitorresults
- sp_dbmmonitorresults_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dbmmonitorresults
- database mirroring [SQL Server], monitoring
ms.assetid: d575e624-7d30-4eae-b94f-5a7b9fa5427e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7af393da5482f8955c8e5eea20c9742ffb269baa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160622"
---
# <a name="sp_dbmmonitorresults-transact-sql"></a>sp_dbmmonitorresults (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce le righe di stato per un database monitorato dalla tabella di stato in cui è archiviata la cronologia di Monitoraggio mirroring del database e consente di scegliere in anticipo se lo stato della procedura verrà aggiornato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_dbmmonitorresults database_name   
   , rows_to_return  
    , update_status   
```  
  
## <a name="arguments"></a>Argomenti  
 *database_name*  
 Specifica il database per cui restituire lo stato di mirroring.  
  
 *rows_to_return*  
 Specifica la quantità di righe restituite:  
  
 0 = Ultima riga  
  
 1 = Righe relative alle ultime due ore  
  
 2 = Righe relative alle ultime quattro ore  
  
 3 = Righe relative alle ultime otto ore  
  
 4 = Righe relative all'ultimo giorno  
  
 5 = Righe relative agli ultimi due giorni  
  
 6 = ultime 100 righe  
  
 7 = ultime 500 righe  
  
 8 = ultime 1.000 righe  
  
 9 = Ultimo milione di righe  
  
 *update_status*  
 Specifica che prima di restituire i risultati, la procedura esegue le operazioni seguenti:  
  
 Se il valore è 0, non aggiorna lo stato del database. I risultati vengono calcolati utilizzando solo le ultime due righe, la cui data dipende dal momento in cui è stato eseguito l'aggiornamento della tabella di stato.  
  
 1 = aggiorna lo stato del database chiamando **sp_dbmmonitorupdate** prima di calcolare i risultati. Tuttavia, se la tabella dello stato è stata aggiornata entro i 15 secondi precedenti o se l'utente non è un membro del ruolo predefinito del server **sysadmin** , **sp_dbmmonitorresults** viene eseguito senza aggiornare lo stato.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Nessuno  
  
## <a name="result-sets"></a>Set di risultati  
 Restituisce il numero richiesto di righe dello stato della cronologia per il database specificato. Ogni riga contiene le informazioni seguenti:  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**database_name**|**sysname**|Nome di un database con mirroring.|  
|**ruolo**|**int**|Ruolo di mirroring corrente dell'istanza del server:<br /><br /> 1 = Database principale<br /><br /> 2 = Database mirror|  
|**mirroring_state**|**int**|Stato del database:<br /><br /> 0 = sospeso<br /><br /> 1 = disconnesso<br /><br /> 2 = Sincronizzazione in corso<br /><br /> 3 = Failover in sospeso<br /><br /> 4 = Sincronizzato|  
|**witness_status**|**int**|Stato di connessione del server di controllo del mirroring nella sessione di mirroring del database. I possibili valori sono i seguenti:<br /><br /> 0 = Sconosciuto<br /><br /> 1 = Connesso<br /><br /> 2 = Disconnesso|  
|**log_generation_rate**|**int**|Quantità di log generati a partire dall'ultimo aggiornamento dello stato di mirroring del database, espressa in kilobyte al secondo.|  
|**unsent_log**|**int**|Dimensioni del log non inviato nella coda di invio nel database principale, espressa in kilobyte.|  
|**send_rate**|**int**|Velocità di invio del log dal database principale al database mirror, espressa in kilobyte al secondo.|  
|**unrestored_log**|**int**|Dimensioni della coda di rollforward nel database mirror, espressa in kilobyte.|  
|**recovery_rate**|**int**|Tempo di rollforward nel database mirror, espresso in kilobyte al secondo.|  
|**transaction_delay**|**int**|Ritardo totale per tutte le transazioni, espresso in millisecondi.|  
|**transactions_per_sec**|**int**|Numero di transazioni al secondo nell'istanza del server principale.|  
|**average_delay**|**int**|Ritardo medio nell'istanza del server principale per ogni transazione causato dal mirroring del database. In modalità a prestazioni elevate, ovvero quando la proprietà SAFETY è impostata su OFF, questo valore è in genere 0.|  
|**time_recorded**|**datetime**|Ora in cui la riga è stata registrata da Monitoraggio mirroring del database. Si tratta dell'ora dell'orologio di sistema del database principale.|  
|**time_behind**|**datetime**|Ora approssimativa dell'orologio di sistema del database principale rispetto al quale è aggiornato il database mirror. Questo valore è significativo solo nell'istanza del server principale.|  
|**local_time**|**datetime**|Ora dell'orologio di sistema nell'istanza locale del server al momento dell'aggiornamento della riga.|  
  
## <a name="remarks"></a>Commenti  
 **sp_dbmmonitorresults** possono essere eseguite solo nel contesto del database **msdb** .  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** o al ruolo predefinito del database **dbm_monitor** nel database **msdb** . Il ruolo **dbm_monitor** consente ai membri di visualizzare lo stato di mirroring del database, ma non di aggiornarlo, ma non di visualizzare né configurare gli eventi di mirroring del database.  
  
> [!NOTE]  
>  La prima volta che si esegue **sp_dbmmonitorupdate** , viene creato il ruolo predefinito del database **dbm_monitor** nel database **msdb** . I membri del ruolo predefinito del server **sysadmin** possono aggiungere qualsiasi utente al ruolo predefinito del database **dbm_monitor** .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite le righe registrate nelle due ore precedenti senza aggiornare lo stato del database.  
  
```  
USE msdb;  
EXEC sp_dbmmonitorresults AdventureWorks2012, 2, 0;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Monitoraggio del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [sp_dbmmonitorchangemonitoring &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md)   
 [sp_dbmmonitoraddmonitoring &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbmmonitoraddmonitoring-transact-sql.md)   
 [sp_dbmmonitordropmonitoring &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbmmonitordropmonitoring-transact-sql.md)   
 [sp_dbmmonitorhelpmonitoring &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpmonitoring-transact-sql.md)   
 [sp_dbmmonitorupdate &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorupdate-transact-sql.md)  
  
  
