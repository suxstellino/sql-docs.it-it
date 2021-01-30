---
description: sp_articlecolumn (Transact-SQL)
title: sp_articlecolumn (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_articlecolumn
- sp_articlecolumn_TSQL
helpviewer_keywords:
- sp_articlecolumn
ms.assetid: 8abaa8c1-d99e-4788-970f-c4752246c577
author: markingmyname
ms.author: maghan
ms.openlocfilehash: da57a5aa4ae6645c7b1b2ff6377b79160fa110c6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203209"
---
# <a name="sp_articlecolumn-transact-sql"></a>sp_articlecolumn (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Consente di specificare le colonne incluse in un articolo in modo da applicare un filtro verticale ai dati in una tabella pubblicata. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_articlecolumn [ @publication = ] 'publication'  
        , [ @article = ] 'article'  
    [ , [ @column = ] 'column' ]  
    [ , [ @operation = ] 'operation' ]  
    [ , [ @refresh_synctran_procs = ] refresh_synctran_procs ]  
    [ , [ @ignore_distributor = ] ignore_distributor ]  
    [ , [ @change_active = ] change_actve ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @internal = ] 'internal' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione contenente l'articolo. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @article = ] 'article'` Nome dell'articolo. *article* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @column = ] 'column'` Nome della colonna da aggiungere o eliminare. *Column* è di **tipo sysname** e il valore predefinito è null. Se il valore è NULL, vengono pubblicate tutte le colonne.  
  
`[ @operation = ] 'operation'` Specifica se aggiungere o eliminare colonne in un articolo. *Operation* è di **tipo nvarchar (5)** e il valore predefinito è Add. **Aggiungi** contrassegna la colonna per la replica. **Drop** deseleziona la colonna.  
  
`[ @refresh_synctran_procs = ] refresh_synctran_procs` Specifica se le stored procedure che supportano le sottoscrizioni ad aggiornamento immediato vengono rigenerate in modo da corrispondere al numero di colonne replicate. *refresh_synctran_procs* è di **bit** e il valore predefinito è **1**. Se è **1**, le stored procedure vengono rigenerate.  
  
`[ @ignore_distributor = ] ignore_distributor` Indica se il stored procedure viene eseguito senza connettersi al server di distribuzione. *ignore_distributor* è di **bit** e il valore predefinito è **0**. Se è **0**, il database deve essere abilitato per la pubblicazione e la cache dell'articolo deve essere aggiornata in modo da riflettere le nuove colonne replicate dall'articolo. Se è **1**, consente di eliminare le colonne dell'articolo per gli articoli che si trovano in un database non pubblicato; deve essere usato solo in situazioni di ripristino.  
  
`[ @change_active = ] change_active` Consente di modificare le colonne delle pubblicazioni con sottoscrizioni. *change_active* è di **tipo int** e il valore predefinito è **0**. Se è **0**, le colonne non vengono modificate. Se è **1**, le colonne possono essere aggiunte o eliminate da articoli attivi con sottoscrizioni.  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Conferma che l'azione eseguita da questo stored procedure potrebbe invalidare uno snapshot esistente. *force_invalidate_snapshot* è di **bit** e il valore predefinito è **0**.  
  
 **0** specifica che le modifiche apportate all'articolo non invalidano lo snapshot. Se la stored procedure rileva che la modifica richiede un nuovo snapshot, viene generato un errore e non viene apportata alcuna modifica.  
  
 **1** specifica che le modifiche apportate all'articolo possono invalidare lo snapshot e, se sono presenti sottoscrizioni che richiedono un nuovo snapshot, consente di contrassegnare lo snapshot esistente come obsoleto e di generare un nuovo snapshot.  
  
 [**@force_reinit_subscription =** ] *force_reinit_subscription*  
 Segnala che l'azione eseguita dalla stored procedure potrebbe richiedere la reinizializzazione delle sottoscrizioni esistenti. *force_reinit_subscription* è di **bit** e il valore predefinito è **0**.  
  
 **0** specifica che le modifiche apportate all'articolo non provocano la reinizializzazione della sottoscrizione. Se la stored procedure rileva che la modifica richiede la reinizializzazione delle sottoscrizioni, viene generato un errore e non viene apportata alcuna modifica. **1** specifica che le modifiche apportate all'articolo provocano la reinizializzazione delle sottoscrizioni esistenti e consente la reinizializzazione della sottoscrizione.  
  
`[ @publisher = ] 'publisher'` Specifica un server di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione non. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  il *server di pubblicazione* non deve essere utilizzato con un server di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione.  
  
`[ @internal = ] 'internal'` Solo per uso interno.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_articlecolumn** viene utilizzata nella replica snapshot e nella replica transazionale.  
  
 È possibile filtrare solo un articolo non sottoscritto utilizzando **sp_articlecolumn**.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_AddTranArticle](../../relational-databases/replication/codesnippet/tsql/sp-articlecolumn-transac_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_articlecolumn**.  
  
## <a name="see-also"></a>Vedere anche  
 [Define an Article](../../relational-databases/replication/publish/define-an-article.md)   
 [Definire e modificare un filtro colonne](../../relational-databases/replication/publish/define-and-modify-a-column-filter.md)   
 [Filtrare i dati pubblicati](../../relational-databases/replication/publish/filter-published-data.md)   
 [sp_addarticle &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_articleview &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_droparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [sp_helparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [sp_helparticlecolumns &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helparticlecolumns-transact-sql.md)   
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
