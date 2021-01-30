---
description: sp_articleview (Transact-SQL)
title: sp_articleview (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_articleview
- sp_articleview_TSQL
helpviewer_keywords:
- sp_articleview
ms.assetid: a3d63fd6-f360-4a2f-8a82-a0dc15f650b3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 014ccf37727534d16fa5b16bc280da5b76dfb76e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203195"
---
# <a name="sp_articleview-transact-sql"></a>sp_articleview (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea la vista che definisce l'articolo pubblicato quando una tabella viene filtrata in senso verticale o orizzontale. Questa vista viene utilizzata come origine filtrata dello schema e dei dati per le tabelle di destinazione. Solo gli articoli non sottoscritti possono essere modificati tramite questa stored procedure. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_articleview [ @publication = ] 'publication'  
        , [ @article = ] 'article'  
    [ , [ @view_name = ] 'view_name']  
    [ , [ @filter_clause = ] 'filter_clause']  
    [ , [ @change_active = ] change_active ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @refreshsynctranprocs = ] refreshsynctranprocs ]  
    [ , [ @internal = ] internal ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione contenente l'articolo. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @article = ] 'article'` Nome dell'articolo. *article* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @view_name = ] 'view_name'` Nome della vista che definisce l'articolo pubblicato. *view_name* è di **tipo nvarchar (386)** e il valore predefinito è null.  
  
`[ @filter_clause = ] 'filter_clause'` Clausola di restrizione (WHERE) che definisce un filtro orizzontale. Quando si specifica la clausola di restrizione, omettere la parola chiave WHERE. *filter_clause* è di tipo **ntext** e il valore predefinito è null.  
  
`[ @change_active = ] change_active` Consente di modificare le colonne delle pubblicazioni con sottoscrizioni. *change_active* è di **tipo int** e il valore predefinito è **0**. Se è **0**, le colonne non vengono modificate. Se è **1**, le visualizzazioni possono essere create o ricreate in articoli attivi con sottoscrizioni.  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Conferma che l'azione eseguita da questo stored procedure potrebbe invalidare uno snapshot esistente. *force_invalidate_snapshot* è di **bit** e il valore predefinito è **0**.  
  
 **0** specifica che le modifiche apportate all'articolo non invalidano lo snapshot. Se la stored procedure rileva che la modifica richiede un nuovo snapshot, viene generato un errore e non viene apportata alcuna modifica.  
  
 **1** specifica che le modifiche apportate all'articolo possono invalidare lo snapshot e, se sono presenti sottoscrizioni che richiedono un nuovo snapshot, consente di contrassegnare lo snapshot esistente come obsoleto e di generare un nuovo snapshot.  
  
`[ @force_reinit_subscription = ] _force_reinit_subscription_` Conferma che l'azione eseguita da questo stored procedure potrebbe richiedere la reinizializzazione delle sottoscrizioni esistenti. *force_reinit_subscription* è un **bit** e il valore predefinito è **0**.  
  
 **0** specifica che le modifiche apportate all'articolo non provocano la reinizializzazione della sottoscrizione. Se la stored procedure rileva che la modifica richiede la reinizializzazione delle sottoscrizioni, viene generato un errore e non viene apportata alcuna modifica.  
  
 **1** specifica che le modifiche apportate all'articolo provocano la reinizializzazione della sottoscrizione esistente e consente la reinizializzazione della sottoscrizione.  
  
`[ @publisher = ] 'publisher'` Specifica un server di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione non. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  il *Server* di pubblicazione non deve essere utilizzato per la pubblicazione da un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] server di pubblicazione.  
  
`[ @refreshsynctranprocs = ] refreshsynctranprocs` Indica se le stored procedure utilizzate per sincronizzare la replica vengono ricreate automaticamente. *refreshsynctranprocs* è di **bit** e il valore predefinito è 1.  
  
 **1** indica che le stored procedure vengono ricreate.  
  
 **0** indica che le stored procedure non vengono ricreate.  
  
`[ @internal = ] internal` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_articleview** crea la vista che definisce l'articolo pubblicato e inserisce l'ID di questa visualizzazione nella colonna **sync_objid** della tabella [sysarticles &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysarticles-transact-sql.md) e inserisce il testo della clausola di restrizione nella colonna **filter_clause** . Se tutte le colonne vengono replicate e non è presente alcun **filter_clause**, il **sync_objid** nella tabella [sysarticles &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysarticles-transact-sql.md) viene impostato sull'ID della tabella di base e l'utilizzo di **sp_articleview** non è obbligatorio.  
  
 Per pubblicare una tabella filtrata verticalmente, ovvero per filtrare le colonne, eseguire prima **sp_addarticle** senza *sync_object* parametro, eseguire [sp_articlecolumn &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) una volta per ogni colonna da replicare (definendo il filtro verticale), quindi eseguire **sp_articleview** per creare la vista che definisce l'articolo pubblicato.  
  
 Per pubblicare una tabella filtrata orizzontalmente, ovvero per filtrare le righe, eseguire [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) senza parametro *Filter* . Eseguire [sp_articlefilter &#40;&#41;Transact-SQL](../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md), fornendo tutti i parametri, inclusi *filter_clause*. Eseguire quindi **sp_articleview**, specificando tutti i parametri, incluso il *filter_clause* identico.  
  
 Per pubblicare una tabella filtrata verticalmente e orizzontalmente, eseguire [sp_addarticle &#40;&#41;Transact-SQL](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) senza *sync_object* o parametri di *filtro* . Eseguire [sp_articlecolumn &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) una volta per ogni colonna da replicare, quindi eseguire sp_articlefilter &#40;&#41;e **sp_articleview** [Transact-SQL](../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md) .  
  
 Se l'articolo dispone già di una vista che definisce l'articolo pubblicato, **sp_articleview** Elimina la vista esistente e ne crea una nuova automaticamente. Se la vista è stata creata manualmente (**digitare** in [sysarticles &#40;&#41;Transact-SQL](../../relational-databases/system-tables/sysarticles-transact-sql.md) è **5**), la vista esistente non viene eliminata.  
  
 Se si crea un filtro personalizzato stored procedure e una vista che definisce l'articolo pubblicato manualmente, non eseguire **sp_articleview**. Specificare invece questi parametri come *filtro* e *sync_object* parametri per [SP_ADDARTICLE &#40;&#41;Transact-SQL](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md), insieme al valore del *tipo* appropriato.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_AddTranArticle](../../relational-databases/replication/codesnippet/tsql/sp-articleview-transact-_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_articleview**.  
  
## <a name="see-also"></a>Vedere anche  
 [Define an Article](../../relational-databases/replication/publish/define-an-article.md)   
 [Definire e modificare un filtro di riga statico](../../relational-databases/replication/publish/define-and-modify-a-static-row-filter.md)   
 [sp_addarticle &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_articlefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_droparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [sp_helparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
