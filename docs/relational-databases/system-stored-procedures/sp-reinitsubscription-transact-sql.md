---
description: sp_reinitsubscription (Transact-SQL)
title: sp_reinitsubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_reinitsubscription
- sp_reinitsubscription_TSQL
helpviewer_keywords:
- sp_reinitsubscription
ms.assetid: d56ae218-6128-4ff9-b06c-749914505c7b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0d8c12b3ca1b27f7560aeff466e51b7f43a9a42e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185659"
---
# <a name="sp_reinitsubscription-transact-sql"></a>sp_reinitsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Contrassegna la sottoscrizione per la reinizializzazione. Questa stored procedure viene eseguita nel server di pubblicazione per sottoscrizioni push.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_reinitsubscription [ [ @publication = ] 'publication' ]  
    [ , [ @article = ] 'article' ]  
        , [ @subscriber = ] 'subscriber'  
    [ , [ @destination_db = ] 'destination_db']  
    [ , [ @for_schema_change = ] 'for_schema_change']  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @ignore_distributor_failure = ] ignore_distributor_failure ]   
    [ , [ @invalidate_snapshot = ] invalidate_snapshot ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e il valore predefinito è all.  
  
`[ @article = ] 'article'` Nome dell'articolo. *article* è di **tipo sysname** e il valore predefinito è all. Per una pubblicazione con aggiornamento immediato, l' *articolo* deve essere **All**; in caso contrario, il stored procedure ignora la pubblicazione e segnala un errore.  
  
`[ @subscriber = ] 'subscriber'` Nome del Sottoscrittore. *Subscriber* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @destination_db = ] 'destination_db'` Nome del database di destinazione. *destination_db* è di **tipo sysname** e il valore predefinito è all.  
  
`[ @for_schema_change = ] 'for_schema_change'` Indica se la reinizializzazione viene eseguita in seguito a una modifica dello schema nel database di pubblicazione. *for_schema_change* è di **bit** e il valore predefinito è 0. Se è **0**, le sottoscrizioni attive per le pubblicazioni che consentono l'aggiornamento immediato vengono riattivate fino a quando l'intera pubblicazione e non solo alcuni degli articoli vengono reinizializzate. Le reinizializzazione viene pertanto avviata in seguito a modifiche dello schema. Se è **1**, le sottoscrizioni attive non vengono riattivate fino a quando non viene eseguita la agente di snapshot.  
  
`[ @publisher = ] 'publisher'` Specifica un server di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione non. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  il server di pubblicazione non deve essere utilizzato per i *Server* di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione.  
  
`[ @ignore_distributor_failure = ] ignore_distributor_failure` Consente la reinizializzazione anche se il server di distribuzione non esiste o è offline. *ignore_distributor_failure* è di **bit** e il valore predefinito è 0. Se è **0**, la reinizializzazione ha esito negativo se il server di distribuzione non esiste o è offline.  
  
`[ @invalidate_snapshot = ] invalidate_snapshot` Invalida lo snapshot della pubblicazione esistente. *invalidate_snapshot* è di **bit** e il valore predefinito è 0. Se è **1**, viene generato un nuovo snapshot per la pubblicazione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_reinitsubscription** viene utilizzata nella replica transazionale.  
  
 **sp_reinitsubscription** non è supportato per la replica transazionale peer-to-peer.  
  
 Per le sottoscrizioni in cui lo snapshot iniziale viene applicato automaticamente e la pubblicazione non consente sottoscrizioni aggiornabili, l'agente snapshot deve essere eseguito al termine di questa stored procedure in modo che vengano preparati i file dello schema e del programma per la copia bulk e gli agenti di distribuzione siano quindi in grado di risincronizzare le sottoscrizioni.  
  
 Per le sottoscrizioni in cui lo snapshot iniziale viene applicato automaticamente e la pubblicazione consente sottoscrizioni aggiornabili, l'agente di distribuzione risincronizza la sottoscrizione utilizzando i file dello schema e del programma per la copia bulk più recenti creati in precedenza dall'agente snapshot. Il agente di distribuzione risincronizza la sottoscrizione immediatamente dopo l'esecuzione dell'utente **sp_reinitsubscription**, se il agente di distribuzione non è occupato. in caso contrario, la sincronizzazione può essere eseguita dopo l'intervallo di messaggi (specificato da agente di distribuzione parametro del prompt dei comandi: **MessageInterval**).  
  
 **sp_reinitsubscription** non ha effetto sulle sottoscrizioni in cui lo snapshot iniziale viene applicato manualmente.  
  
 Per risincronizzare le sottoscrizioni anonime di una pubblicazione, passare **All** o null come *Sottoscrittore*.  
  
 La replica transazionale supporta la reinizializzazione della sottoscrizione a livello di articolo. Lo snapshot dell'articolo viene riapplicato nel Sottoscrittore durante la successiva sincronizzazione dopo che l'articolo è stato contrassegnato per la reinizializzazione. Se esistono articoli dipendenti sottoscritti dallo stesso Sottoscrittore, tuttavia, la riapplicazione dello snapshot nell'articolo potrebbe avere esito negativo, a meno che anche gli articoli dipendenti della pubblicazione non vengano reinizializzati automaticamente in particolari circostanze:  
  
-   Se il comando preliminare eseguito sull'articolo è 'drop', gli articoli di viste associate a schema e stored procedure associate a schema dell'oggetto di base di tale articolo devono essere anch'essi contrassegnati per la reinizializzazione.  
  
-   Se l'opzione dello schema dell'articolo include script dell'integrità referenziale dichiarata nelle chiavi primarie, gli articoli le cui tabelle di base includono relazioni di chiave esterna con le tabelle di base dell'articolo reinizializzato devono essere anch'essi contrassegnati per la reinizializzazione.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_reinittranpushsub](../../relational-databases/replication/codesnippet/tsql/sp-reinitsubscription-tr_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** , i membri del ruolo predefinito del database **db_owner** o l'autore della sottoscrizione possono eseguire **sp_reinitsubscription**.  
  
## <a name="see-also"></a>Vedere anche  
 [Reinizializzare una sottoscrizione](../../relational-databases/replication/reinitialize-a-subscription.md)   
 [Reinizializzare le sottoscrizioni](../../relational-databases/replication/reinitialize-subscriptions.md)   
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
