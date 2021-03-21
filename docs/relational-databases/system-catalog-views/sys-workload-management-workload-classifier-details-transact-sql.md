---
description: sys.workload_management_workload_classifier_details (Transact-SQL)
title: sys.workload_management_workload_classifier_details (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/05/2019
ms.prod: sql
ms.technology: system-objects
ms.prod_service: synapse-analytics
ms.reviewer: jrasnick
ms.topic: reference
dev_langs:
- TSQL
author: ronortloff
ms.author: rortloff
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: b8b2a43c84f2aaa294fd68931472f0cf1f7d5339
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104747801"
---
# <a name="sysworkload_management_workload_classifier_details-transact-sql"></a>sys.workload_management_workload_classifier_details (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

  Restituisce i dettagli per ogni classificatore.  
  
|Nome colonna|Tipo di dati|Descrizione|Range|  
|-----------------|---------------|-----------------|-----------|
|classifier_id|**int**|ID del classificatore.  Non ammette i valori Null.|
|classifier_type|**sysname**|Joinable to [sys.workload_management_workload_classifiers](sys-workload-management-workload-classifiers-transact-sql.md).|`membername`</br>`wlm_label`</br>`wlm_context`</br>`start_time`</br>`end_time`|
|classifier_value|**sysname**|Valore del classificatore. Non ammette i valori Null.||

## <a name="permissions"></a>Autorizzazioni

È richiesta l'autorizzazione VIEW SERVER STATE.

## <a name="next-steps"></a>Passaggi successivi
  
Per un elenco di tutte le viste del catalogo per analisi sinapsi di Azure e data warehouse parallele, vedere [analisi delle sinapsi di Azure e viste del catalogo data warehouse parallele](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md). Per creare un classificatore del carico di lavoro, vedere [creare un classificatore del carico di lavoro](../../t-sql/statements/create-workload-classifier-transact-sql.md). Per altre informazioni sulla classificazione del carico di lavoro, vedere [classificazione](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification) del carico [di lavoro](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification) di analisi delle sinapsi di Azure
