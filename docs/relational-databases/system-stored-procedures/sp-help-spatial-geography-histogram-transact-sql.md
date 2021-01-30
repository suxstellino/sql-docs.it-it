---
description: sp_help_spatial_geography_histogram (Transact-SQL)
title: sp_help_spatial_geography_histogram (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_spatial_geography_histogram_TSQL
- sp_help_spatial_geography_histogram
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_spatial_geography_histogram
ms.assetid: 5c5bd319-055d-4cd6-8c5a-06354cc056cc
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3554c8b385a14f588c4f7f8dd6248ba36ad7b16d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212070"
---
# <a name="sp_help_spatial_geography_histogram-transact-sql"></a>sp_help_spatial_geography_histogram (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Semplifica l'impostazione delle chiavi dei parametri di griglia per un indice spaziale.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_help_spatial_geography_histogram [ @tabname =] 'tabname'   
     [ , [ @colname = ] 'columnname' ]   
     [ , [ @resolution = ] 'resolution' ]  
     [ , [ @sample = ] 'tablesample' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @tabname = ] 'tabname'` Nome completo o non qualificato della tabella per la quale è stato specificato l'indice spaziale.  
  
 Le virgolette sono necessarie solo se viene specificata una tabella qualificata. Nel caso di un nome completo, ovvero contenente un nome di database, il nome del database deve corrispondere a quello del database corrente. *TabName* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @colname = ] 'columnname'` Nome della colonna spaziale specificata. *ColumnName* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @resolution = ] 'resolution'` Risoluzione del rettangolo di delimitazione. I valori validi sono compresi tra 10 e 5000. la *risoluzione* è di **tinyint** e non prevede alcun valore predefinito.  
  
`[ @sample = ] 'sample'` Percentuale della tabella utilizzata. I valori validi sono compresi tra 0 e 100. *TABLESAMPLE* è un valore **float**. Il valore predefinito è 100.  
  
## <a name="property-valuereturn-value"></a>Valore proprietà/Valore restituito  
 Viene restituito un valore di tabella. Nella griglia seguente viene descritto il contenuto delle colonne della tabella.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**cellid**|**int**|Rappresenta l'ID univoco di ciascuna cella, con un conteggio iniziale di 1.|  
|**cella**|**geography**|Poligono rettangolare che rappresenta ciascuna cella. La forma della cella è identica alla forma della cella utilizzata per l'indicizzazione spaziale.|  
|**row_count**|**bigint**|Indica il numero di oggetti spaziali che toccano o contengono la cella.|  
  
## <a name="permissions"></a>Autorizzazioni  
 L'utente deve essere un membro del ruolo **public** . È necessario disporre dell'autorizzazione READ ACCESS per il server e l'oggetto.  
  
## <a name="remarks"></a>Commenti  
 Nella scheda spaziale di SSMS viene illustrata una rappresentazione grafica dei risultati. È possibile eseguire una query sui risultati rispetto alla finestra spaziale per ottenere un numero approssimativo di risultati.  
  
> [!NOTE]  
>  Gli oggetti nella tabella potrebbero interessare più di una cella, pertanto la somma delle celle nella tabella potrebbe essere maggiore del numero di oggetti effettivi.  
  
 Il rettangolo di delimitazione per il tipo **geography** è l'intero globo.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene chiamato  **sp_help_spatial_geography_histogram** sulla `Person.Address` tabella del [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] database.  
  
```  
EXEC sp_help_spatial_geography_histogram @tabname = Person.Address, @colname = SpatialLocation, @resolution = 64, @sample = 30;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di indice spaziale &#40;&#41;Transact-SQL ](./spatial-index-stored-procedures-arguments-and-properties.md)  
  
