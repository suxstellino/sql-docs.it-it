---
description: sp_check_dynamic_filters (Transact-SQL)
title: sp_check_dynamic_filters (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- dynamic_filters_TSQL
- sp_check_TSQL
- check
- sp_check_dynamic filter
- check_TSQL
- filters_TSQL
- check_dynamic_filters_TSQL
- dynamic filters
- filters
- check dynamic filters
- sp_check_dynamic filter_TSQL
- sp_check_for_sync_trigger_TSQL
- sp_check
helpviewer_keywords:
- sp_check_dynamic_filters
ms.assetid: dd7760db-a3a5-460f-bd97-b8d436015e19
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1f458f3dcf629ee6f77b8237edb0a9defa516b8f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158960"
---
# <a name="sp_check_dynamic_filters-transact-sql"></a>sp_check_dynamic_filters (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Visualizza informazioni sulle proprietà dei filtri di riga con parametri per una pubblicazione, specificando in particolare le funzioni utilizzate per generare una partizione di dati filtrati per una pubblicazione e se la pubblicazione consente l'utilizzo di partizioni pre-calcolate. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_check_dynamic_filters [ @publication = ] 'publication'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**can_use_partition_groups**|**bit**|Indica se la pubblicazione è idonea per l'utilizzo di partizioni pre-calcolate. dove **1** indica che è possibile utilizzare le partizioni pre-calcolate e **0** indica che non è possibile utilizzarle.|  
|**has_dynamic_filters**|**bit**|Indica se nella pubblicazione è stato definito almeno un filtro di riga con parametri. dove **1** indica che sono presenti uno o più filtri di riga con parametri e **0** indica che non esistono filtri dinamici.|  
|**dynamic_filters_function_list**|**nvarchar (500)**|Elenco delle funzioni utilizzate per filtrare gli articoli di una pubblicazione, separate con un punto e virgola.|  
|**validate_subscriber_info**|**nvarchar (500)**|Elenco delle funzioni utilizzate per filtrare gli articoli di una pubblicazione, separate con un segno più (+).|  
|**uses_host_name**|**bit**|Se la funzione [HOST_NAME ()](../../t-sql/functions/host-name-transact-sql.md) viene utilizzata nei filtri di riga con parametri, dove **1** indica che questa funzione viene utilizzata per l'applicazione di filtri dinamici.|  
|**uses_suser_sname**|**bit**|Se la funzione [SUSER_SNAME ()](../../t-sql/functions/suser-sname-transact-sql.md) viene utilizzata nei filtri di riga con parametri, dove **1** indica che questa funzione viene utilizzata per l'applicazione di filtri dinamici.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_check_dynamic_filters** viene utilizzata nella replica di tipo merge.  
  
 Se è stata definita una pubblicazione per l'utilizzo di partizioni pre-calcolate, **sp_check_dynamic_filters** verifica eventuali violazioni delle restrizioni delle partizioni pre-calcolate. Se viene rilevata una qualsiasi violazione, viene restituito un errore. Per altre informazioni, vedere [Ottimizzare le prestazioni dei filtri con parametri con le partizioni pre-calcolate](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md).  
  
 Se una pubblicazione è stata definita in modo da includere filtri di riga con parametri ma non viene rilevato alcun filtro, viene restituito un errore.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_check_dynamic_filters**.  
  
## <a name="see-also"></a>Vedere anche  
 [Gestire le partizioni per una pubblicazione di tipo merge con filtri con parametri](../../relational-databases/replication/publish/manage-partitions-for-a-merge-publication-with-parameterized-filters.md)   
 [sp_check_join_filter &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-check-join-filter-transact-sql.md)   
 [sp_check_subset_filter &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-check-subset-filter-transact-sql.md)  
  
  
