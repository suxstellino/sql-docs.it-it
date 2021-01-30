---
description: sp_dbmmonitorhelpalert (Transact-SQL)
title: sp_dbmmonitorhelpalert (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dbmmonitorhelpalert_TSQL
- sp_dbmmonitorhelpalert
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dbmmonitorhelpalert
- database mirroring [SQL Server], monitoring
ms.assetid: 43911660-b4e4-4934-8c02-35221160aaec
author: markingmyname
ms.author: maghan
ms.openlocfilehash: edcf719e5778ebe7cd2f224c14f53552a2f28325
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212374"
---
# <a name="sp_dbmmonitorhelpalert-transact-sql"></a>sp_dbmmonitorhelpalert (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sulle soglie di avviso su una o tutte le misurazioni delle prestazioni di monitoraggio del mirroring del database.  
 
  ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_dbmmonitorhelpalert database_name   
    [ , alert_id ]   
```  
  
## <a name="arguments"></a>Argomenti  
 *database_name*  
 Specifica il database.  
  
 [ *alert_id* ]  
 Valore intero che identifica l'avviso da restituire. Se questo argomento viene omesso, vengono restituiti tutti gli avvisi ma non il periodo di memorizzazione.  
  
 Per restituire un avviso specifico, specificare uno dei valori seguenti:  
  
|Valore|Misurazione delle prestazioni|Valore soglia avvisi|  
|-----------|------------------------|-----------------------|  
|1|Transazione non inviata meno recente|Specifica la quantità di transazioni, espressa in minuti, che può accumularsi nella coda di invio prima che venga generato un avviso nell'istanza del server principale. Questo avviso consente di quantificare il rischio potenziale di perdita dei dati in termini di tempo ed è particolarmente rilevante per la modalità a prestazioni elevate. L'avviso risulta tuttavia utile anche per la modalità a sicurezza elevata quando il mirroring viene sospeso in seguito alla disconnessione dei partner.|  
|2|Log non inviato|Specifica la quantità di log non inviati, espressa in kilobyte (KB), che può accumularsi prima che venga generato un avviso nell'istanza del server principale. Questo avviso consente di quantificare il rischio potenziale di perdita dei dati in termini di KB ed è particolarmente rilevante per la modalità a prestazioni elevate. L'avviso risulta tuttavia utile anche per la modalità a sicurezza elevata quando il mirroring viene sospeso in seguito alla disconnessione dei partner.|  
|3|Log non ripristinato|Specifica la quantità di log non ripristinati, espressa in kilobyte (KB), che può accumularsi prima che venga generato un avviso nell'istanza del server mirror. Questo avviso consente di misurare il tempo di failover. Il *tempo di failover* corrisponde essenzialmente al tempo necessario al server mirror precedente per eseguire il rollforward di tutti i log rimanenti nella propria coda di rollforward, più un breve tempo aggiuntivo.|  
|4|Overhead commit mirror|Specifica il ritardo medio per transazione, espresso in millisecondi, che è consentito prima che venga generato un avviso nell'istanza del server principale. Questo ritardo rappresenta la quantità di overhead generato mentre l'istanza del server principale è in attesa che l'istanza del server mirror scriva il record di log della transazione nella coda di rollforward. Questo valore è rilevante solo nella modalità a sicurezza elevata.|  
|5|Periodo di memorizzazione|Metadati che controllano per quanto tempo vengono conservate le righe della tabella dello stato di mirroring del database.|  
  
 Per informazioni sugli ID evento corrispondenti agli avvisi, vedere [usare valori di soglia avvisi e avvisi sulle metriche delle prestazioni di mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md).  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Nessuno  
  
## <a name="result-sets"></a>Set di risultati  
 Per ogni avviso restituito, restituisce una riga contenente le colonne seguenti:  
  
|Colonna|Tipo di dati|Descrizione|  
|------------|---------------|-----------------|  
|**alert_id**|**int**|Nella tabella seguente sono elencati i valori **alert_id** per ogni metrica delle prestazioni e l'unità di misura della metrica visualizzata nel set di risultati della **sp_dbmmonitorresults** :|  
|**threshold**|**int**|Valore soglia per l'avviso. Se quando si aggiorna lo stato di mirroring viene restituito un valore che supera tale soglia, viene immessa una voce nel registro eventi di Windows. Questo valore è espresso in kilobyte, minuti o millisecondi, a seconda dell'avviso. Se la soglia non è impostata, il valore è NULL.<br /><br /> **Nota:** Per visualizzare i valori correnti, eseguire il [sp_dbmmonitorresults](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md) stored procedure.|  
|**abilitato**|**bit**|0 = L'evento è disabilitato.<br /><br /> 1 = L'evento è abilitato.<br /><br /> **Nota:** Il periodo di memorizzazione è sempre abilitato.|  
  
|Valore|Misurazione delle prestazioni|Unità|  
|-----------|------------------------|----------|  
|1|Transazione non inviata meno recente|Minuti|  
|2|Log non inviato|KB|  
|3|Log non ripristinato|KB|  
|4|Overhead commit mirror|Millisecondi|  
|5|Periodo di memorizzazione|Ore|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene restituita una riga tramite cui viene indicato se è abilitato un avviso per la metrica delle prestazioni della transazione non inviata meno recente nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
EXEC sp_dbmmonitorhelpalert AdventureWorks2012, 1 ;  
```  
  
 Nell'esempio seguente viene restituita una riga per ogni metrica delle prestazioni che indica se questa è abilitata nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
EXEC sp_dbmmonitorhelpalert AdventureWorks2012;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Monitoraggio del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [sp_dbmmonitorchangealert &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangealert-transact-sql.md)   
 [sp_dbmmonitorchangemonitoring &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md)   
 [sp_dbmmonitordropalert &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbmmonitordropalert-transact-sql.md)   
 [sp_dbmmonitorupdate &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbmmonitorupdate-transact-sql.md)   
 [sp_dbmmonitorhelpmonitoring &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpmonitoring-transact-sql.md)   
 [sp_dbmmonitorresults &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md)  
  
  
