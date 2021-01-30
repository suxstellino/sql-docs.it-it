---
description: sp_addmergefilter (Transact-SQL)
title: sp_addmergefilter (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergefilter
- sp_addmergefilter_TSQL
helpviewer_keywords:
- sp_addmergefilter
ms.assetid: 4c118cb1-2008-44e2-a797-34b7dc34d6b1
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 836f493a3ae4ca6d3f17928af0d5faa367bdd1b4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192339"
---
# <a name="sp_addmergefilter-transact-sql"></a>sp_addmergefilter (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Aggiunge un nuovo filtro di merge per la creazione di una partizione in base a un join con un'altra tabella. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addmergefilter [ @publication = ] 'publication'   
        , [ @article = ] 'article'   
        , [ @filtername = ] 'filtername'   
        , [ @join_articlename = ] 'join_articlename'   
        , [ @join_filterclause = ] join_filterclause  
    [ , [ @join_unique_key = ] join_unique_key ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
    [ , [ @filter_type = ] filter_type ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione in cui viene aggiunto il filtro di merge. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @article = ] 'article'` Nome dell'articolo in cui viene aggiunto il filtro di merge. *article* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @filtername = ] 'filtername'` Nome del filtro. *FilterName* è un parametro obbligatorio. *FilterName* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @join_articlename = ] 'join_articlename'` Articolo padre al quale l'articolo figlio, specificato da *article*, deve essere unito utilizzando la clausola join specificata da *join_filterclause* per determinare le righe dell'articolo figlio che soddisfano il criterio di filtro del filtro di merge. *join_articlename* è di **tipo sysname** e non prevede alcun valore predefinito. È necessario che l'articolo sia presente nella pubblicazione fornita dalla *pubblicazione*.  
  
`[ @join_filterclause = ] join_filterclause` Clausola join che deve essere utilizzata per unire in join l'articolo figlio specificato dall' *articolo e l'* articolo padre specificato da *join_article* per determinare le righe che qualificano il filtro di merge. *join_filterclause* è di **tipo nvarchar (1000)**.  
  
`[ @join_unique_key = ] join_unique_key` Specifica se il join tra l'articolo *dell'articolo figlio e l'* articolo padre *join_article* è uno-a-molti, uno-a-uno, molti-a-uno o molti-a-molti. *join_unique_key* è di **tipo int** e il valore predefinito è 0. **0** indica un join molti-a-uno o molti-a-molti. **1** indica un join uno-a-uno o uno-a-molti. Questo valore è **1** quando le colonne di join formano una chiave univoca in *join_article* o se *join_filterclause* tra una chiave esterna in un *articolo* e una chiave primaria in *join_article*.  
  
> [!CAUTION]  
>  Impostare questo parametro su **1** solo se si dispone di un vincolo nella colonna di join nella tabella sottostante per l'articolo padre che garantisce l'univocità. Se *join_unique_key* è impostato su **1** in modo errato, è possibile che si verifichi la non convergenza dei dati.  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Conferma che l'azione eseguita da questo stored procedure potrebbe invalidare uno snapshot esistente. *force_invalidate_snapshot* è un **bit** e il valore predefinito è **0**.  
  
 **0** specifica che le modifiche apportate all'articolo di merge non saranno valide per lo snapshot. Se la stored procedure rileva che la modifica richiede un nuovo snapshot, viene generato un errore e non viene apportata alcuna modifica.  
  
 **1** specifica che le modifiche apportate all'articolo di merge potrebbero invalidare lo snapshot e, se sono presenti sottoscrizioni che richiedono un nuovo snapshot, consente di contrassegnare lo snapshot esistente come obsoleto e di generare un nuovo snapshot.  
  
`[ @force_reinit_subscription = ] force_reinit_subscription` Conferma che l'azione eseguita da questo stored procedure potrebbe richiedere la reinizializzazione delle sottoscrizioni esistenti. *force_reinit_subscription* è di **bit** e il valore predefinito è 0.  
  
 **0** specifica che le modifiche apportate all'articolo di merge non comporteranno la reinizializzazione della sottoscrizione. Se la stored procedure rileva che la modifica richiede la reinizializzazione delle sottoscrizioni, viene generato un errore e non viene apportata alcuna modifica.  
  
 **1** specifica che le modifiche apportate all'articolo di merge causeranno la reinizializzazione delle sottoscrizioni esistenti e consentirà di eseguire la reinizializzazione della sottoscrizione.  
  
`[ @filter_type = ] filter_type` Specifica il tipo di filtro da aggiungere. *filter_type* è di **tinyint**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|Solo filtro di join. Necessario per supportare i [!INCLUDE[ssEW](../../includes/ssew-md.md)] sottoscrittori.|  
|**2**|Solo relazione tra record logici.|  
|**3**|Filtro di join e relazione tra record logici.|  
  
 Per altre informazioni, vedere [Raggruppare modifiche alle righe correlate con record logici](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_addmergefilter** viene utilizzata nella replica di tipo merge.  
  
 **sp_addmergefilter** può essere utilizzato solo con gli articoli di tabella. Gli articoli di vista e di vista indicizzata non sono supportati.  
  
 Questa procedura può inoltre essere utilizzata per aggiungere una relazione logica tra due articoli che possono essere uniti o meno tramite un filtro di join. *filter_type* viene utilizzato per specificare se il filtro di merge aggiunto è un filtro join, una relazione logica o entrambi.  
  
 Per utilizzare record logici, è necessario che la pubblicazione e gli articoli soddisfino alcuni requisiti. Per altre informazioni, vedere [Raggruppare modifiche alle righe correlate con record logici](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
 Questa opzione viene generalmente utilizzata per gli articoli che includono un riferimento di chiave esterna a una chiave primaria su una tabella pubblicata e tale tabella include un filtro definito nell'articolo corrispondente. Il subset di righe della chiave primaria viene utilizzato per determinare le righe di chiave esterna da replicare nel Sottoscrittore.  
  
 Non è possibile aggiungere un filtro di join tra due articoli pubblicati se le tabelle di origine per entrambi gli articoli condividono lo stesso nome di oggetto tabella. In questo caso, anche se entrambe le tabelle appartengono a schemi diversi e sono caratterizzate da nomi di articolo univoci, la creazione del filtro di join avrà esito negativo.  
  
 In caso di utilizzo di un filtro di riga con parametri e un filtro di join in un articolo di tabella, la replica determina se una riga appartiene a una partizione del Sottoscrittore Questa operazione viene eseguita valutando la funzione di filtro o il filtro join (usando l'operatore [or](../../t-sql/language-elements/or-transact-sql.md) ), anziché valutare l'intersezione delle due condizioni (usando l'operatore [and](../../t-sql/language-elements/and-transact-sql.md) ).  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_addmergefilter](../../relational-databases/replication/codesnippet/tsql/sp-addmergefilter-transa_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_addmergefilter**.  
  
## <a name="see-also"></a>Vedere anche  
 [Define an Article](../../relational-databases/replication/publish/define-an-article.md)   
 [Definizione e modifica di un filtro di join tra articoli di merge](../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)   
 [Join Filters](../../relational-databases/replication/merge/join-filters.md)   
 [sp_changemergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergefilter-transact-sql.md)   
 [sp_dropmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergefilter-transact-sql.md)   
 [sp_helpmergefilter &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpmergefilter-transact-sql.md)   
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
