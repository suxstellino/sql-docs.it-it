---
description: sp_dropmergepullsubscription (Transact-SQL)
title: sp_dropmergepullsubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropmergepullsubscription
- sp_dropmergepullsubscription_TSQL
helpviewer_keywords:
- sp_dropmergepullsubscription
ms.assetid: 9301dd80-72f7-4adb-9b13-87e7f9114248
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6d0ee785012033001e9c458749eecc7921ef3fe3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208231"
---
# <a name="sp_dropmergepullsubscription-transact-sql"></a>sp_dropmergepullsubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elimina una sottoscrizione pull di tipo merge. Questa stored procedure viene eseguita nel database di sottoscrizione del Sottoscrittore.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_dropmergepullsubscription [ @publication= ] 'publication'   
        , [ @publisher= ] 'publisher'   
        , [ @publisher_db= ] 'publisher_db'   
    [ , [ @reserved= ] 'reserved' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e il valore predefinito è null. Questo parametro è obbligatorio. Specificare il valore **All** per rimuovere le sottoscrizioni di tutte le pubblicazioni  
  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e il valore predefinito è null. Questo parametro è obbligatorio.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database del server di pubblicazione. *publisher_db* è di **tipo sysname** e il valore predefinito è null. Questo parametro è obbligatorio.  
  
`[ @reserved = ] 'reserved'` È riservato per usi futuri. *riservato* è di **bit** e il valore predefinito è **0**.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_dropmergepullsubscription** viene utilizzata nella replica di tipo merge.  
  
 **sp_dropmergepullsubscription** elimina la agente di merge per questa sottoscrizione pull di tipo merge, sebbene il agente di merge non venga creato in **sp_addmergepullsubscription**.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_dropmergepullsubscription](../../relational-databases/replication/codesnippet/tsql/sp-dropmergepullsubscrip_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o dell'utente che ha creato la sottoscrizione pull di tipo merge possono eseguire **sp_dropmergepullsubscription**. Il ruolo predefinito del database **db_owner** è in grado di eseguire **sp_dropmergepullsubscription** solo se l'utente che ha creato la sottoscrizione pull di tipo merge appartiene a questo ruolo.  
  
## <a name="see-also"></a>Vedere anche  
 [Eliminare una sottoscrizione pull](../../relational-databases/replication/delete-a-pull-subscription.md)   
 [sp_addmergepullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md)   
 [sp_changemergepullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changemergepullsubscription-transact-sql.md)   
 [sp_dropmergesubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropmergesubscription-transact-sql.md)   
 [sp_helpmergepullsubscription &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpmergepullsubscription-transact-sql.md)  
  
  
