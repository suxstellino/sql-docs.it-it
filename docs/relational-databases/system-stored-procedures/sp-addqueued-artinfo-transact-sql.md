---
description: sp_addqueued_artinfo (Transact-SQL)
title: sp_addqueued_artinfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addqueued_artinfo
- sp_addqueued_artinfo_TSQL
helpviewer_keywords:
- sp_addqueued_artinfo
ms.assetid: decdb6eb-3dcd-4053-a21d-fd367c3fbafb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c0899ff490b553471fc766413fdf0cca27955fa0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206660"
---
# <a name="sp_addqueued_artinfo-transact-sql"></a>sp_addqueued_artinfo (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  
  
> [!IMPORTANT]  
>  È consigliabile utilizzare la procedura [sp_script_synctran_commands](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md) anziché **sp_addqueued_artinfo**. [sp_script_synctran_commands](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md) genera uno script che contiene le chiamate **sp_addqueued_artinfo** e **sp_addsynctrigger** .  
  
 Crea la tabella [MSsubscription_articles](../../relational-databases/system-tables/mssubscription-articles-transact-sql.md) nel Sottoscrittore utilizzata per tenere traccia delle informazioni sulla sottoscrizione degli articoli (aggiornamento in coda e aggiornamento immediato con aggiornamento in coda come failover). Questa stored procedure viene eseguita nel database di sottoscrizione del Sottoscrittore.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addqueued_artinfo [ @artid= ] 'artid'  
        , [ @article= ] 'article'  
        , [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'  
        , [ @dest_table= ] 'dest_table'  
        , [ @owner = ] 'owner'  
        , [ @cft_table= ] 'cft_table'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @artid = ] 'artid'` Nome dell'ID dell'articolo. *artid* è di **tipo int** e non prevede alcun valore predefinito  
  
`[ @article = ] 'article'` Nome dell'articolo da inserire nello script. *article* è di **tipo sysname** e non prevede alcun valore predefinito  
  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database del server di pubblicazione. *publisher_db* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publication = ] 'publication'` Nome della pubblicazione da inserire nello script. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @dest_table = ] _'dest_table'` Nome della tabella di destinazione. *dest_table* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
 [**@owner =** ] **'**_proprietario_**'**  
 Proprietario della sottoscrizione. *owner* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @cft_table = ] 'cft_table'` Nome della tabella dei conflitti ad aggiornamento in coda per l'articolo. *cft_table* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_addqueued_artinfo** viene utilizzato dal agente di distribuzione come parte dell'inizializzazione della sottoscrizione. In genere, non viene eseguita dagli utenti, ma può risultare utile se l'utente deve configurare una sottoscrizione in modo manuale.  
  
 [sp_script_synctran_commands](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md) anziché **sp_addqueued_artinfo**.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_addqueued_artinfo**.  
  
## <a name="see-also"></a>Vedere anche  
 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [sp_script_synctran_commands &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md)   
 [MSsubscription_articles &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/mssubscription-articles-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
