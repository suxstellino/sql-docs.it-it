---
description: sp_validatemergesubscription (Transact-SQL)
title: sp_validatemergesubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_validatemergesubscription
- sp_validatemergesubscription_TSQL
helpviewer_keywords:
- sp_validatemergesubscription
ms.assetid: d73ad03c-e5b3-4606-a0ee-7d75e12762a6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 15ebc99d11b0ff009d80f769ea9fbfad60f11c76
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201814"
---
# <a name="sp_validatemergesubscription-transact-sql"></a>sp_validatemergesubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Esegue la convalida della sottoscrizione specificata. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_validatemergesubscription [@publication=] 'publication'  
        , [ @subscriber = ] 'subscriber'  
        , [ @subscriber_db = ] 'subscriber_db'  
        , [ @level = ] level  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @subscriber = ] 'subscriber'` Nome del Sottoscrittore. *Subscriber* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @subscriber_db = ] 'subscriber_db'` Nome del database di sottoscrizione. *subscriber_db* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @level = ] 'level'` Tipo di convalida da eseguire. il *livello* è **tinyint** e non prevede alcun valore predefinito. I possibili valori del livello sono i seguenti.  
  
|Valore del livello|Descrizione|  
|-----------------|-----------------|  
|**1**|Convalida solo tramite conteggio delle righe.|  
|**2**|Convalida mediante conteggio delle righe e checksum.|  
|**3**|Convalida mediante conteggio delle righe e checksum binario.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_validatemergesubscription** viene utilizzata nella replica di tipo merge.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_validatemergesubscription**.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [Convalida dei dati replicati](../../relational-databases/replication/validate-data-at-the-subscriber.md)   
 [sp_validatemergepublication &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-validatemergepublication-transact-sql.md)  
  
  
