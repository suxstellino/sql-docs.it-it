---
description: fn_syscollector_get_execution_stats (Transact-SQL)
title: fn_syscollector_get_execution_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_syscollector_get_execution_stats
- fn_syscollector_get_execution_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_syscollector_get_execution_stats function
ms.assetid: 793ad72c-a992-4a8d-8584-bcb6b3b476f1
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 47a90958d7de27e087df3768949a5defa148c8f4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097492"
---
# <a name="fn_syscollector_get_execution_stats-transact-sql"></a>fn_syscollector_get_execution_stats (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce statistiche dettagliate sul set di raccolta o sul pacchetto, incluso il numero di righe di errore registrato dall'attività Flusso di dati di un pacchetto. Un'attività Flusso di dati è un componente di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] tramite cui vengono elaborati i dati. I dati sono in formato relazionale, pertanto sono presenti un set di dati di input e uno di output costituiti da righe.  
  
 Le statistiche vengono calcolate dalle voci nella vista syscollector_execution_stats.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
fn_syscollector_get_execution_stats ( log_id )  
```  
  
## <a name="arguments"></a>Argomenti  
 *log_id*  
 Identificatore univoco locale del log di esecuzione. *log_id* è di **tipo int**.  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|avg_row_count_in|**int**|Numero medio di righe passate nelle attività del flusso di dati del pacchetto.<br /><br /> Nota: un'attività flusso di dati è un [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] componente che elabora i dati. I dati sono in formato relazionale, pertanto è presente un set di dati di input costituito da righe. Si tratta del numero di righe immesse nell'attività. Dopo la trasformazione dei dati, viene restituito l'output rappresentato da un set di risultati costituito da righe. L'attività Flusso di dati trasforma i dati e restituisce un set di risultati costituito da righe. L'output è costituito dal numero di righe restituite dall'attività.|  
|min_row_count_in|**int**|Numero minimo di righe passate nelle attività del flusso di dati del pacchetto.|  
|max_row_count_in|**int**|Numero massimo di righe passate nelle attività del flusso di dati del pacchetto.|  
|avg_row_count_out|**int**|Numero medio di righe uscite dalle attività del flusso di dati del pacchetto.|  
|min_row_count_out|**int**|Numero minimo di righe restituite dalle attività Flusso di dati del pacchetto.|  
|max_row_count_out|**int**|Numero massimo di righe restituite dalle attività Flusso di dati del pacchetto.|  
|avg_duration|**int**|Tempo medio, in millisecondi, impiegato nel componente flusso di dati del pacchetto.|  
|min_duration|**int**|Tempo minimo, in millisecondi, impiegato nel componente flusso di dati del pacchetto.|  
|max_duration|**int**|Tempo massimo, in millisecondi, impiegato nel componente flusso di dati del pacchetto.|  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede SELECT per **dc_operator**.  
  
## <a name="see-also"></a>Vedere anche  
 [syscollector_execution_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/syscollector-execution-stats-transact-sql.md)   
 [Raccolta dati](../../relational-databases/data-collection/data-collection.md)  
  
  
