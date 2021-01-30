---
description: sp_helptracertokenhistory (Transact-SQL)
title: sp_helptracertokenhistory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helptracertokenhistory_TSQL
- sp_helptracertokenhistory
helpviewer_keywords:
- sp_helptracertokenhistory
ms.assetid: 96910d1c-be76-43eb-9c93-4477e6761749
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f701a1681281bfb63cf6c39ba42869ac83df204e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192878"
---
# <a name="sp_helptracertokenhistory-transact-sql"></a>sp_helptracertokenhistory (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Restituisce informazioni dettagliate sulla latenza per i token di traccia specificati. Per ogni Sottoscrittore, viene restituita una riga. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione o nel database di distribuzione del server di distribuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helptracertokenhistory [ @publication = ] 'publication'   
        , [ @tracer_id = ] tracer_id  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @publisher_db = ] 'publisher_db' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione in cui è stato inserito il token di traccia. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @tracer_id = ] tracer_id` ID del token di traccia nella [MStracer_tokens &#40;tabella&#41;Transact-SQL ](../../relational-databases/system-tables/mstracer-tokens-transact-sql.md) per cui vengono restituite le informazioni sulla cronologia. *tracer_id* è di **tipo int** e non prevede alcun valore predefinito.  
  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e il valore predefinito è null.  
  
> [!NOTE]
>  Questo parametro deve essere specificato solo per i [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Publisher non.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database di pubblicazione. *publisher_db* è di **tipo sysname** e il valore predefinito è null. Questo parametro viene ignorato se la stored procedure viene eseguita nel server di pubblicazione.  
  
## <a name="result-set"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**distributor_latency**|**bigint**|Numero di secondi che intercorrono tra il commit del record del token di traccia nel server di pubblicazione e il commit nel server di distribuzione.|  
|**Sottoscrittore**|**sysname**|Nome del Sottoscrittore che ha ricevuto il token di traccia.|  
|**subscriber_db**|**sysname**|Nome del database di sottoscrizione in cui è stato inserito il record del token di traccia.|  
|**subscriber_latency**|**bigint**|Numero di secondi che intercorrono tra il commit del record del token di traccia nel server di distribuzione e il commit nel Sottoscrittore.|  
|**overall_latency**|**bigint**|Numero di secondi che intercorrono tra il commit del record del token di traccia nel server di pubblicazione e il commit nel Sottoscrittore.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_helptracertokenhistory** viene utilizzata nella replica transazionale.  
  
 Eseguire [sp_helptracertokens &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helptracertokens-transact-sql.md) per ottenere un elenco dei token di traccia per la pubblicazione.  
  
 Un valore NULL nel set di risultati indica che non è stato possibile calcolare le statistiche relative alla latenza, perché il token di traccia non è stato ricevuto nel server di distribuzione o in uno dei Sottoscrittori.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_tracertokens](../../relational-databases/replication/codesnippet/tsql/sp-helptracertokenhistor_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** , il **db_owner** ruolo predefinito del database nel database di pubblicazione o **db_owner** ruoli predefiniti del database o **replmonitor** nel database di distribuzione possono eseguire **sp_helptracertokenhistory**.  
  
## <a name="see-also"></a>Vedere anche  
 [Misurare la latenza e convalidare le connessioni per la replica transazionale](../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)   
 [sp_deletetracertokenhistory &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-deletetracertokenhistory-transact-sql.md)  
  
  
