---
description: syspolicy_policy_execution_history_details (Transact-SQL)
title: syspolicy_policy_execution_history_details (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- syspolicy_policy_execution_history_details
- syspolicy_policy_execution_history_details_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syspolicy_policy_execution_history_details view
ms.assetid: 97ef6573-5e8b-4ba5-8ae0-7901e79a9683
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: d10e65b6a701acaabc80418d59da019250acf24b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171829"
---
# <a name="syspolicy_policy_execution_history_details-transact-sql"></a>syspolicy_policy_execution_history_details (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Visualizza le espressioni di condizione eseguite, le destinazioni delle espressioni, il risultato di ciascuna esecuzione e le informazioni su eventuali errori. Nella tabella seguente sono descritte le colonne contenute nella vista syspolicy_execution_history_details.  
  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|detail_id|**bigint**|Identificatore del record. Ogni record rappresenta il tentativo di valutare o applicare un'espressione della condizione nei criteri. Se applicato a più destinazioni, ogni condizione disporrà di un record di dettaglio per ciascuna destinazione.|  
|history_id|**bigint**|Identificatore dell'evento di cronologia. Ogni evento della cronologia rappresenta un tentativo di esecuzione dei criteri. Poiché una condizione può disporre di molte espressioni e molte destinazioni, un identificatore history_id può creare molti record di dettaglio. Utilizzare la colonna history_id per unire in join questa visualizzazione alla visualizzazione [syspolicy_policy_execution_history](../../relational-databases/system-catalog-views/syspolicy-policy-execution-history-transact-sql.md) .|  
|target_query_expression|**nvarchar(max)**|Destinazione del criterio e della vista syspolicy_policy_execution_history.|  
|execution_date|**datetime**|Data e ora in cui è stato creato il record di dettaglio.|  
|result|**bit**|Esito positivo o negativo della valutazione di questa destinazione e dell'espressione della condizione:<br /><br /> 0 (esito positivo) o 1 (esito negativo)|  
|result_detail|**nvarchar(max)**|Messaggio del risultato. Disponibile solo se fornito dal facet.|  
|exception_message|**nvarchar(max)**|Messaggio generato da un'eventuale eccezione.|  
|exception|**nvarchar(max)**|Descrizione dell'eventuale eccezione.|  
  
## <a name="remarks"></a>Commenti  
 Durante la risoluzione di problemi relativi alla gestione basata su criteri, eseguire una query sulla vista syspolicy_policy_execution_history_details per stabilire quali combinazioni di destinazione ed espressione di condizione hanno avuto esito negativo, in quali casi hanno avuto esito negativo e per analizzare gli errori correlati.  
  
 Nella query seguente la vista `syspolicy_policy_execution_history_details` viene combinata con le viste `syspolicy_policy_execution_history_details` e `syspolicy_policies` per visualizzare il nome dei criteri, il nome della condizione e le informazioni sugli errori.  
  
```  
SELECT Pol.name AS Policy,   
Cond.name AS Condition,   
PolHistDet.target_query_expression,   
PolHistDet.execution_date,   
PolHistDet.result,   
PolHistDet.result_detail,   
PolHistDet.exception_message,   
PolHistDet.exception   
FROM msdb.dbo.syspolicy_policies AS Pol  
JOIN msdb.dbo.syspolicy_conditions AS Cond  
    ON Pol.condition_id = Cond.condition_id  
JOIN msdb.dbo.syspolicy_policy_execution_history AS PolHist  
    ON Pol.policy_id = PolHist.policy_id  
JOIN msdb.dbo.syspolicy_policy_execution_history_details AS PolHistDet  
    ON PolHist.history_id = PolHistDet.history_id  
WHERE PolHistDet.result = 0 ;  
```  
  
## <a name="permissions"></a>Autorizzazioni  
 È necessaria l'appartenenza al ruolo PolicyAdministratorRole nel database msdb.  
  
## <a name="see-also"></a>Vedere anche  
 [Amministrazione di server tramite la gestione basata su criteri](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)   
 [Viste di Gestione basata su criteri &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/policy-based-management-views-transact-sql.md)  
  
  
