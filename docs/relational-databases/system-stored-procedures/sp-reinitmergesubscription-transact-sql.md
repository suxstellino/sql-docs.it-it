---
description: sp_reinitmergesubscription (Transact-SQL)
title: sp_reinitmergesubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_reinitmergesubscription_TSQL
- sp_reinitmergesubscription
helpviewer_keywords:
- sp_reinitmergesubscription
ms.assetid: 249a4048-e885-48e0-a92a-6577f59de751
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5d4a16d71453d08fca6ab496cc103e43edbc160e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185671"
---
# <a name="sp_reinitmergesubscription-transact-sql"></a>sp_reinitmergesubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contrassegna una sottoscrizione di tipo merge per la reinizializzazione alla successiva esecuzione dell'agente di merge. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_reinitmergesubscription [ [ @publication = ] 'publication'  
    [ , [ @subscriber = ] 'subscriber'  
    [ , [ @subscriber_db = ] 'subscriber_db'  
    [ , [ @upload_first = ] 'upload_first'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e il valore predefinito è **All**.  
  
`[ @subscriber = ] 'subscriber'` Nome del Sottoscrittore. *Subscriber* è di **tipo sysname** e il valore predefinito è **All**.  
  
`[ @subscriber_db = ] 'subscriber_db'` Nome del database Sottoscrittore. *subscriber_db* è di **tipo sysname** e il valore predefinito è **All**.  
  
`[ @upload_first = ] 'upload_first'` Indica se le modifiche nel Sottoscrittore vengono caricate prima della reinizializzazione della sottoscrizione. *upload_first* è di **tipo nvarchar (5)** e il valore predefinito è false. Se **true**, le modifiche vengono caricate prima della reinizializzazione della sottoscrizione. Se **false**, le modifiche non vengono caricate.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_reinitmergesubscription** viene utilizzata nella replica di tipo merge.  
  
 **sp_reinitmergesubscription** possibile chiamare dal server di pubblicazione per reinizializzare le sottoscrizioni di tipo merge. È consigliabile rieseguire anche l'agente snapshot.  
  
 Se si aggiunge, elimina o modifica un filtro con parametri, le modifiche in sospeso nel Sottoscrittore non possono essere caricate nel server di pubblicazione durante la reinizializzazione. Per caricare le modifiche in sospeso, sincronizzare tutte le sottoscrizioni prima di modificare il filtro.  
  
## <a name="examples"></a>Esempi  

### <a name="a-reinitialize-the-push-subscription-and-lose-pending-changes"></a>R. Reinizializzare la sottoscrizione push e perdere le modifiche in sospeso

 [!code-sql[HowTo#sp_reinitmergepushsub](../../relational-databases/replication/codesnippet/tsql/sp-reinitmergesubscripti_1.sql)]  
  
### <a name="b-reinitialize-the-push-subscription-and-upload-pending-changes"></a>B. Reinizializzare la sottoscrizione push e caricare le modifiche in sospeso
 [!code-sql[HowTo#sp_reinitmergepushsubwithupload](../../relational-databases/replication/codesnippet/tsql/sp-reinitmergesubscripti_2.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_reinitmergesubscription**.  
  
## <a name="see-also"></a>Vedere anche  
 [Reinizializzare le sottoscrizioni](../../relational-databases/replication/reinitialize-subscriptions.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
