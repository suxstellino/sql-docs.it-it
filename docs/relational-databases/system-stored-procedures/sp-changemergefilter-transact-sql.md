---
description: sp_changemergefilter (Transact-SQL)
title: sp_changemergefilter (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changemergefilter_TSQL
- sp_changemergefilter
helpviewer_keywords:
- sp_changemergefilter
ms.assetid: e08fdfdd-d242-4e85-817b-9f7a224fe567
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8081fc9a95452d68540acf3b2a5a412910f75219
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159843"
---
# <a name="sp_changemergefilter-transact-sql"></a>sp_changemergefilter (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Consente di modificare alcune proprietà del filtro di merge. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_changemergefilter [ @publication= ] 'publication'  
        , [ @article= ] 'article'  
        , [ @filtername= ] 'filtername'  
        , [ @property= ] 'property'  
        , [ @value= ] 'value'  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @article = ] 'article'` Nome dell'articolo. *article* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @filtername = ] 'filtername'` Nome corrente del filtro. *FilterName* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @property = ] 'property'` Nome della proprietà da modificare. *Property* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @value = ] 'value'` Nuovo valore per la proprietà specificata. *value* è di **tipo nvarchar (1000)** e non prevede alcun valore predefinito.  
  
 Nella tabella seguente vengono descritte le proprietà degli articoli e i valori corrispondenti.  
  
|Proprietà|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**filter_type**|**1**|Filtro join.<br /><br /> Questa opzione è necessaria per supportare i Sottoscrittori [!INCLUDE[ssEW](../../includes/ssew-md.md)].|  
||**2**|Relazione tra record logici.|  
||**3**|Il filtro join è anche una relazione tra record logici.|  
|**NomeFiltro**||Nome del filtro.|  
|**join_articlename**||Nome dell'articolo di join.|  
|**join_filterclause**||Clausola di filtro.|  
|**join_unique_key**|**true**|Il join è basato su una chiave univoca.|  
||**false**|Il join non è basato su una chiave univoca.|  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Conferma che l'azione eseguita da questo stored procedure potrebbe invalidare uno snapshot esistente. *force_invalidate_snapshot* è un **bit** e il valore predefinito è **0**.  
  
 **0** specifica che le modifiche apportate all'articolo di merge non invalidano lo snapshot. Se la stored procedure rileva che la modifica richiede un nuovo snapshot, viene generato un errore e non viene apportata alcuna modifica.  
  
 **1** indica che le modifiche apportate all'articolo di merge potrebbero invalidare lo snapshot e, se sono presenti sottoscrizioni che richiedono un nuovo snapshot, consente di contrassegnare lo snapshot esistente come obsoleto e di generare un nuovo snapshot.  
  
`[ @force_reinit_subscription = ] force_reinit_subscription` Conferma che l'azione eseguita da questo stored procedure potrebbe richiedere la reinizializzazione delle sottoscrizioni esistenti. *force_reinit_subscription* è un **bit** e il valore predefinito è **0**.  
  
 **0** specifica che le modifiche apportate all'articolo di merge non provocano la reinizializzazione della sottoscrizione. Se la stored procedure rileva che la modifica richiede la reinizializzazione delle sottoscrizioni esistenti, viene generato un errore e non viene apportata alcuna modifica.  
  
 **1** indica che le modifiche apportate all'articolo di merge comportano la reinizializzazione delle sottoscrizioni esistenti e consente la reinizializzazione della sottoscrizione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_changemergefilter** viene utilizzata nella replica di tipo merge.  
  
 Per modificare il filtro in un articolo di merge, è necessario ricreare lo snapshot, se disponibile. Questa operazione viene eseguita impostando il **\@ force_invalidate_snapshot** su **1**. È inoltre necessario reinizializzare le eventuali sottoscrizioni esistenti dell'articolo. Questa operazione viene eseguita impostando il **\@ force_reinit_subscription** su **1**.  
  
 Per utilizzare record logici, è necessario che la pubblicazione e gli articoli soddisfino alcuni requisiti. Per altre informazioni, vedere [Raggruppare modifiche alle righe correlate con record logici](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_changemergefilter**.  
  
## <a name="see-also"></a>Vedere anche  
 [Modificare le proprietà di pubblicazioni e articoli](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [sp_addmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md)   
 [sp_dropmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergefilter-transact-sql.md)   
 [sp_helpmergefilter &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpmergefilter-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
