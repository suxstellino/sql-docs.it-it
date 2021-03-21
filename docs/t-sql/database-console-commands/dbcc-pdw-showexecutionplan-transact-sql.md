---
description: DBCC PDW_SHOWEXECUTIONPLAN (Transact-SQL)
title: DBCC PDW_SHOWEXECUTIONPLAN (Transact-SQL)
ms.custom: ''
ms.date: 07/16/2017
ms.prod: sql
ms.technology: data-warehouse
ms.prod_service: synapse-analytics, pdw
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
author: pmasl
ms.author: umajay
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 29b7af17720e83fe6ceb338b1fe9370e754080b8
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754531"
---
# <a name="dbcc-pdw_showexecutionplan-transact-sql"></a>DBCC PDW_SHOWEXECUTIONPLAN (Transact-SQL)

[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

Consente di visualizzare il piano di esecuzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per una query in esecuzione in uno specifico nodo di controllo o di calcolo [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. Consente di risolvere i problemi relativi alle prestazioni di query quando le query vengono eseguite sui nodi di calcolo e di controllo.
  
Dopo il rilevamento di problemi di prestazioni delle query per le query [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SMP in esecuzione nei nodi di calcolo, esistono diversi modi per migliorare le prestazioni. Le possibili soluzioni per migliorare le prestazioni delle query sui nodi di calcolo includono la creazione di statistiche multicolonna, la creazione di indici non cluster o l'uso di hint per la query.
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
Sintassi per [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]:

```syntaxsql
DBCC PDW_SHOWEXECUTIONPLAN ( distribution_id, spid )  
[ ; ]  
```  

Sintassi per [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]:
  
```syntaxsql
DBCC PDW_SHOWEXECUTIONPLAN ( pdw_node_id, spid )  
[ ; ]  
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Argomenti  
 *distribution_id*  
 Identificatore per la distribuzione in cui è in esecuzione il piano di query. È un valore intero e non può essere Null. Il valore deve essere compreso tra 1 e 60. Usato quando la destinazione è [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].  
  
 *pdw_node_id*  
 Identificatore per il nodo in cui è in esecuzione il piano di query. È un valore intero e non può essere Null. Usato quando la destinazione è un'appliance.  
  
 *spid*  
 Identificatore per la sessione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui è in esecuzione il piano di query. È un valore intero e non può essere Null.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CONTROL per [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].  
  
È richiesta l'autorizzazione VIEW-SERVER-STATE nell'appliance.
  
## <a name="examples-sssdw"></a>Esempi: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]  
  
### <a name="a-dbcc-pdw_showexecutionplan-basic-syntax"></a>R. Sintassi di base DBCC PDW_SHOWEXECUTIONPLAN  
 Durante l'esecuzione su un'istanza [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], modificare la query precedente per selezionare anche distribution_id.  
  
```sql
SELECT [sql_spid], [pdw_node_id], [request_id], [dms_step_index], [type], [start_time], [end_time], [status], [distribution_id]  
FROM sys.dm_pdw_dms_workers   
WHERE [status] <> 'StepComplete' and [status] <> 'StepError'  
order by request_id, [dms_step_index];  
```  
  
Verrà restituito il valore spid per ogni distribuzione effettivamente in esecuzione. Se si vuole sapere quale distribuzione 1 era in esecuzione nella sessione 375, eseguire il comando seguente.
  
```sql
DBCC PDW_SHOWEXECUTIONPLAN ( 1, 375 );  
```  

## <a name="examples-sspdw"></a>Esempi: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
### <a name="b-dbcc-pdw_showexecutionplan-basic-syntax"></a>B. Sintassi di base DBCC PDW_SHOWEXECUTIONPLAN  
 La query che è in esecuzione da troppo tempo sta eseguendo un'operazione del piano di query DMS o un'operazione del piano di query SQL.  
  
Se la query sta eseguendo un'operazione del piano di query DMS, è possibile usare la query seguente per recuperare un elenco degli ID di nodo e di sessione per i passaggi che non sono stati completati.
  
```sql
SELECT [sql_spid], [pdw_node_id], [request_id], [dms_step_index], [type], [start_time], [end_time], [status]   
FROM sys.dm_pdw_dms_workers   
WHERE [status] <> 'StepComplete' and [status] <> 'StepError'  
AND pdw_node_id = 201001   
ORDER BY request_id, [dms_step_index], [distribution_id];  
```  
  
In base ai risultati della query precedente, usare sql_spid e pdw_node_id come parametri per DBCC PDW_SHOWEXECUTIONPLAN. Il comando seguente illustra ad esempio il piano di esecuzione per pdw_node_id 201001 e sql_spid 375.
  
```sql
DBCC PDW_SHOWEXECUTIONPLAN ( 201001, 375 );  
```  

## <a name="see-also"></a>Vedere anche

- [DBCC PDW_SHOWPARTITIONSTATS &#40;Transact-SQL&#41;](dbcc-pdw-showpartitionstats-transact-sql.md)  
- [DBCC PDW_SHOWSPACEUSED &#40;Transact-SQL&#41;](dbcc-pdw-showspaceused-transact-sql.md)
