---
description: sp_check_subset_filter (Transact-SQL)
title: sp_check_subset_filter (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_check_TSQL
- sp_check_subset_filter
- filter
- subset
- subset_TSQL
- sp_check
- sp_check_subset_filter_TSQL
helpviewer_keywords:
- sp_check_subset_filter
ms.assetid: 525cfcfc-f317-478d-ba84-72e62285f160
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6d311670fca8b5514057bd122980e8647566240e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174538"
---
# <a name="sp_check_subset_filter-transact-sql"></a>sp_check_subset_filter (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Questa stored procedure viene utilizzata per controllare una clausola di filtro in qualsiasi tabella per determinarne la validità per la tabella e per restituire informazioni sul filtro specificato, incluso se il filtro può essere utilizzato con partizioni pre-calcolate. Questa stored procedure viene eseguita nel database contenente la pubblicazione nel server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_check_subset_filter [ @filtered_table = ] 'filtered_table'  
        , [ @subset_filterclause = ] 'subset_filterclause'  
    [ , [ @has_dynamic_filters = ] has_dynamic_filters OUTPUT ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @filtered_table = ] 'filtered_table'` Nome di una tabella filtrata. *filtered_table* è di **tipo nvarchar (400)** e non prevede alcun valore predefinito.  
  
`[ @subset_filterclause = ] 'subset_filterclause'` Clausola di filtro sottoposta a test. *subset_filterclause* è di **tipo nvarchar (1000)** e non prevede alcun valore predefinito.  
  
`[ @has_dynamic_filters = ] has_dynamic_filters` Indica se la clausola di filtro è un filtro di riga con parametri. *has_dynamic_filters* è di **bit** e il valore predefinito è null ed è un parametro di output. Restituisce un valore pari a **1** quando la clausola di filtro è un filtro di riga con parametri.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**can_use_partition_groups**|**bit**|Indica se la pubblicazione è idonea per l'utilizzo di partizioni pre-calcolate. dove **1** indica che è possibile utilizzare le partizioni pre-calcolate e **0** indica che non è possibile utilizzarle.|  
|**has_dynamic_filters**|**bit**|Indica se la clausola di filtro fornita include almeno un filtro di riga con parametri. dove **1** indica che viene utilizzato un filtro di riga con parametri e **0** indica che tale funzione non viene utilizzata.|  
|**dynamic_filters_function_list**|**nvarchar (500)**|Elenca le funzioni nella clausola di filtro che filtrano dinamicamente un articolo. Ogni funzione è separata da un punto e virgola.|  
|**uses_host_name**|**bit**|Se la funzione [HOST_NAME ()](../../t-sql/functions/host-name-transact-sql.md) viene utilizzata nella clausola di filtro, dove **1** indica che questa funzione è presente.|  
|**uses_suser_sname**|**bit**|Se la funzione [SUSER_SNAME ()](../../t-sql/functions/suser-sname-transact-sql.md) viene utilizzata nella clausola di filtro, dove **1** indica che questa funzione è presente.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_check_subset_filter** viene utilizzata nella replica di tipo merge.  
  
 **sp_check_subset_filter** possono essere eseguite su qualsiasi tabella anche se la tabella non è pubblicata. Questa stored procedure può essere utilizzata per verificare una clausola di filtro prima di definire un articolo filtrato.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_check_subset_filter**.  
  
## <a name="see-also"></a>Vedere anche  
 [Ottimizzare le prestazioni dei filtri con parametri con le partizioni pre-calcolate](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md)  
  
  
