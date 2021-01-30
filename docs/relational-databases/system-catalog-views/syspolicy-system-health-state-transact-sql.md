---
description: syspolicy_system_health_state (Transact-SQL)
title: syspolicy_system_health_state (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- syspolicy_system_health_state_TSQL
- syspolicy_system_health_state
dev_langs:
- TSQL
helpviewer_keywords:
- syspolicy_system_health_state view
ms.assetid: 00815106-9fe4-481d-a9e1-a256101887f4
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 245ce67d7ee94eca468c9cda775cff15f0b520a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180750"
---
# <a name="syspolicy_system_health_state-transact-sql"></a>syspolicy_system_health_state (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Visualizza una riga per ogni combinazione di criteri di gestione basata su criteri ed espressione della query di destinazione. Utilizzare la vista syspolicy_system_health_state per controllare a livello di programmazione lo stato di integrità criteri del server. Nella tabella seguente sono descritte le colonne contenute nella vista syspolicy_system_health_state.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|health_state_id|**bigint**|Identificatore del record di stato di integrità criteri.|  
|policy_id|**int**|Identificatore dei criteri.|  
|last_run_date|**datetime**|Data e ora dell'ultima esecuzione dei criteri.|  
|target_query_expression_with_id|**nvarchar (400)**|Espressione di destinazione, con valori assegnati a variabili di identità che definiscono la destinazione rispetto alla quale vengono valutati i criteri.|  
|target_query_expression|**nvarchar(max)**|Espressione tramite cui viene definita la destinazione rispetto alla quale vengono valutati i criteri.|  
|result|**bit**|Stato di integrità di questa destinazione in relazione ai criteri:<br /><br /> 0 = esito negativo<br /><br /> 1 = esito positivo|  
  
## <a name="remarks"></a>Commenti  
 Nella vista syspolicy_system_health_state viene visualizzato lo stato di integrità più recente dell'espressione della query di destinazione per i criteri attivi (abilitati). Nella pagina Esplora oggetti e Dettagli Esplora oggetti di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] vengono aggregati gli stati di integrità criteri di questa vista per mostrare lo stato di integrità critica.  
  
## <a name="permissions"></a>Autorizzazioni  
 È necessaria l'appartenenza al ruolo PolicyAdministratorRole nel database msdb.  
  
## <a name="see-also"></a>Vedere anche  
 [Amministrazione di server tramite la gestione basata su criteri](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)   
 [Viste di Gestione basata su criteri &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/policy-based-management-views-transact-sql.md)  
  
  
