---
description: sp_marksubscriptionvalidation (Transact-SQL)
title: sp_marksubscriptionvalidation (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_marksubscriptionvalidation
- sp_marksubscriptionvalidation_TSQL
helpviewer_keywords:
- sp_marksubscriptionvalidation
ms.assetid: e68fe0b9-5993-4880-917a-b0f661f8459b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1fadb2c21aacab18741576d5809c28df989a67d6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185321"
---
# <a name="sp_marksubscriptionvalidation-transact-sql"></a>sp_marksubscriptionvalidation (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contrassegna la transazione aperta corrente come transazione di convalida a livello di sottoscrizione per il Sottoscrittore specificato. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_marksubscriptionvalidation [ @publication = ] 'publication'  
        , [ @subscriber = ] 'subscriber'  
        , [ @destination_db = ] 'destination_db'  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @subscriber = ] 'subscriber'` Nome del Sottoscrittore. *Subscriber* è di tipo sysname e non prevede alcun valore predefinito.  
  
`[ @destination_db = ] 'destination_db'` Nome del database di destinazione. *destination_db* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publisher = ] 'publisher'` Specifica un server di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pubblicazione non. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]  
>  il *Server* di pubblicazione non deve essere utilizzato per una pubblicazione che appartiene a un server di pubblicazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_marksubscriptionvalidation** viene utilizzata nella replica transazionale.  
  
 **sp_marksubscriptionvalidation** non supporta i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sottoscrittori non.  
  
 Per i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Publisher non è possibile eseguire **sp_marksubscriptionvalidation** dall'interno di una transazione esplicita. perché le transazioni esplicite non sono supportate attraverso la connessione al server collegato utilizzata per l'accesso al server di pubblicazione.  
  
 **sp_marksubscriptionvalidation** deve essere utilizzato insieme a [Sp_article_validation &#40;&#41;Transact-SQL](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md), specificando il valore **1** per *subscription_level*, e può essere utilizzato con altre chiamate a **sp_marksubscriptionvalidation** per contrassegnare la transazione aperta corrente per altri Sottoscrittori.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_marksubscriptionvalidation**.  
  
## <a name="example"></a>Esempio  
 È possibile applicare la query seguente al database di pubblicazione per inviare comandi di convalida a livello di sottoscrizione. Tali comandi vengono quindi intercettati dagli agenti di distribuzione dei Sottoscrittori specificati. Si noti che la prima transazione convalida l'articolo '**ART1**', mentre la seconda transazione convalida '**ART2**'. Si noti inoltre che le chiamate a **sp_marksubscriptionvalidation** e [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md) sono state incapsulate in una transazione. Si consiglia una sola chiamata a [sp_article_validation &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md) per transazione. Questo perché [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md) utilizza un blocco di tabella condiviso per la tabella di origine per la durata della transazione. Le transazioni brevi consentono di ottenere la massima concorrenza.  
  
```  
begin tran  
  
exec sp_marksubscriptionvalidation @publication = 'pub1',  
 @subscriber = 'Sub', @destination_db = 'SubDB'  
  
exec sp_marksubscriptionvalidation @publication = 'pub1',  
 @subscriber = 'Sub2', @destination_db = 'SubDB'  
  
exec sp_article_validation @publication = 'pub1', @article = 'art1',  
 @rowcount_only = 0, @full_or_fast = 0, @shutdown_agent = 0,  
 @subscription_level = 1  
  
commit tran  
  
begin tran  
  
exec sp_marksubscriptionvalidation @publication = 'pub1',  
 @subscriber = 'Sub', @destination_db = 'SubDB'  
  
exec sp_marksubscriptionvalidation @publication = 'pub1',  
 @subscriber = 'Sub2', @destination_db = 'SubDB'  
  
exec sp_article_validation @publication = 'pub1', @article = 'art2',  
 @rowcount_only = 0, @full_or_fast = 0, @shutdown_agent = 0,  
 @subscription_level = 1  
  
commit tran  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Convalida dei dati replicati](../../relational-databases/replication/validate-data-at-the-subscriber.md)  
  
  
