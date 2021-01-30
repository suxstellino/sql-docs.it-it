---
description: sp_droparticle (Transact-SQL)
title: sp_droparticle (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_droparticle_TSQL
- sp_droparticle
helpviewer_keywords:
- sp_droparticle
ms.assetid: 09fec594-53f4-48a5-8edb-c50731c7adb2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 42b89405015235d5c469b2443995ea5038e3cd5f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200687"
---
# <a name="sp_droparticle-transact-sql"></a>sp_droparticle (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Elimina un articolo da una pubblicazione snapshot o transazionale. Non è possibile rimuovere un articolo se esistono una o più sottoscrizioni per tale articolo. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_droparticle [ @publication= ] 'publication'  
        , [ @article= ] 'article'  
    [ , [ @ignore_distributor = ] ignore_distributor ]  
    [ , [ @force_invalidate_snapshot= ] force_invalidate_snapshot ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @from_drop_publication = ] from_drop_publication ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione contenente l'articolo da eliminare. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @article = ] 'article'` Nome dell'articolo da eliminare. *article* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @ignore_distributor = ] ignore_distributor` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Conferma che l'azione eseguita da questo stored procedure potrebbe invalidare uno snapshot esistente. *force_invalidate_snapshot* è di **bit** e il valore predefinito è **0**.  
  
 **0** specifica che le modifiche apportate all'articolo non invalidano lo snapshot. Se la stored procedure rileva che la modifica richiede un nuovo snapshot, viene generato un errore e non viene apportata alcuna modifica.  
  
 **1** specifica che le modifiche apportate all'articolo possono invalidare lo snapshot e, se sono presenti sottoscrizioni che richiedono un nuovo snapshot, consente di contrassegnare lo snapshot esistente come obsoleto e di generare un nuovo snapshot.  
  
`[ @publisher = ] 'publisher'` Specifica un server di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione non. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  Impossibile utilizzare *Publisher* quando si modificano le proprietà degli articoli in un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] server di pubblicazione.  
  
`[ @from_drop_publication = ] from_drop_publication` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_droparticle** viene utilizzata per la replica snapshot e transazionale.  
  
 Per gli articoli con filtro orizzontale, **sp_droparticle** controlla la colonna di **tipo** dell'articolo nella tabella [&#41;Transact-SQL di sysarticles &#40;](../../relational-databases/system-tables/sysarticles-transact-sql.md) per determinare se è necessario eliminare anche una vista o un filtro. Se sono disponibili viste o filtri generati in modo automatico, questi vengono eliminati insieme all'articolo. Le viste e i filtri creati in modo manuale non vengono eliminati.  
  
 L'esecuzione di **sp_droparticle** per eliminare un articolo da una pubblicazione non comporta la rimozione dell'oggetto dal database di pubblicazione o dell'oggetto corrispondente dal database di sottoscrizione. Utilizzare `DROP <object>` per rimuovere manualmente questi oggetti, se necessario.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_droparticle](../../relational-databases/replication/codesnippet/tsql/sp-droparticle-transact-_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_droparticle**.  
  
## <a name="see-also"></a>Vedere anche  
 [Eliminare un articolo](../../relational-databases/replication/publish/delete-an-article.md)   
 [Aggiungere ed eliminare articoli in pubblicazioni esistenti](../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md)   
 [sp_addarticle &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_helparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [sp_helparticlecolumns &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helparticlecolumns-transact-sql.md)   
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
