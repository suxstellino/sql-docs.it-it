---
description: sp_dropmergepublication (Transact-SQL)
title: sp_dropmergepublication (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropmergepublication
- sp_dropmergepublication_TSQL
helpviewer_keywords:
- sp_dropmergepublication
ms.assetid: 9e1cb96e-5889-4f97-88cd-f60cf313ce68
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 35024724255eea714de42f8fb3a974188730fcef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208222"
---
# <a name="sp_dropmergepublication-transact-sql"></a>sp_dropmergepublication (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elimina una pubblicazione di tipo merge e l'agente snapshot associato. Prima di eliminare una pubblicazione di tipo merge, è necessario eliminare tutte le sottoscrizioni. Gli articoli della pubblicazione vengono eliminati automaticamente. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_dropmergepublication [ @publication= ] 'publication'   
    [ , [ @ignore_distributor = ] ignore_distributor ]   
    [ , [ @reserved = ] reserved ]  
    [ , [ @ignore_merge_metadata = ] ignore_merge_metadata ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione da eliminare. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito. Se **tutti**, tutte le pubblicazioni di tipo merge esistenti vengono rimosse e il agente di snapshot processo associato. Se si specifica un valore specifico per la *pubblicazione*, vengono eliminate solo la pubblicazione e il processo agente di snapshot associato.  
  
`[ @ignore_distributor = ] ignore_distributor` Utilizzato per eliminare una pubblicazione senza eseguire attività di pulizia nel server di distribuzione. *ignore_distributor* è di **bit** e il valore predefinito è **0**. Questo parametro viene utilizzato anche quando si reinstalla il server di distribuzione.  
  
`[ @reserved = ] reserved` È riservato per usi futuri. *riservato* è di **bit** e il valore predefinito è **0**.  
  
`[ @ignore_merge_metadata = ] ignore_merge_metadata` Solo per uso interno.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_dropmergepublication** viene utilizzata nella replica di tipo merge.  
  
 **sp_dropmergepublication** Elimina in modo ricorsivo tutti gli articoli associati a una pubblicazione e quindi Elimina la pubblicazione stessa. Non è possibile rimuovere una pubblicazione per cui esistono una o più sottoscrizioni. Per informazioni su come rimuovere le sottoscrizioni, vedere [eliminare una sottoscrizione push](../../relational-databases/replication/delete-a-push-subscription.md) ed [eliminare una sottoscrizione pull](../../relational-databases/replication/delete-a-pull-subscription.md).  
  
 L'esecuzione di **sp_dropmergepublication** per eliminare una pubblicazione non comporta la rimozione degli oggetti pubblicati dal database di pubblicazione o degli oggetti corrispondenti dal database di sottoscrizione. Usare DROP \<object> per rimuovere questi oggetti manualmente, se necessario.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_dropmergepublication](../../relational-databases/replication/codesnippet/tsql/sp-dropmergepublication-_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_dropmergepublication**.  
  
## <a name="see-also"></a>Vedere anche  
 [Eliminare una pubblicazione](../../relational-databases/replication/publish/delete-a-publication.md)   
 [sp_addmergepublication &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)   
 [sp_changemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)   
 [sp_helpmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)   
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
