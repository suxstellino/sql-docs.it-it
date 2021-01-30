---
description: sp_helptracertokens (Transact-SQL)
title: sp_helptracertokens (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helptracertokens
- sp_helptracertokens_TSQL
helpviewer_keywords:
- sp_helptracertokens
ms.assetid: 61f27234-531d-4b37-8fa3-fe4c32e6f521
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 504f45e1f4f993b03d7117df6e073c7cd8115b26
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192863"
---
# <a name="sp_helptracertokens-transact-sql"></a>sp_helptracertokens (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Restituisce una riga per ogni token di traccia che è stato inserito in una pubblicazione per determinare la latenza. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione o nel database di distribuzione del server di distribuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helptracertokens [ @publication = ] 'publication'   
    [ , [ @publisher = ] 'publisher' ]   
    [ , [ @publisher_db = ] 'publisher_db' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione in cui sono stati inseriti i token di traccia. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]
>  Questo parametro deve essere specificato solo per i [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Publisher non.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database di pubblicazione. *publisher_db* è di **tipo sysname** e il valore predefinito è null. Questo parametro viene ignorato se la stored procedure viene eseguita nel server di pubblicazione.  
  
## <a name="result-set"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**tracer_id**|**int**|Identifica un record di token di traccia.|  
|**publisher_commit**|**datetime**|Data e ora in cui è stato eseguito il commit del record del token nel database di pubblicazione del server di pubblicazione.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_helptracertokens** viene utilizzata nella replica transazionale.  
  
 **sp_helptracertokens** viene utilizzato per ottenere gli ID del token di traccia durante l'esecuzione di [sp_helptracertokenhistory &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptracertokenhistory-transact-sql.md).  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_tracertokens](../../relational-databases/replication/codesnippet/tsql/sp-helptracertokens-tran_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** , il **db_owner** ruolo predefinito del database nel database di pubblicazione o **db_owner** ruoli predefiniti del database o **replmonitor** nel database di distribuzione possono eseguire **sp_helptracertokenhistory**.  
  
## <a name="see-also"></a>Vedere anche  
 [Misurare la latenza e convalidare le connessioni per la replica transazionale](../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)   
 [sp_deletetracertokenhistory &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-deletetracertokenhistory-transact-sql.md)  
  
  
