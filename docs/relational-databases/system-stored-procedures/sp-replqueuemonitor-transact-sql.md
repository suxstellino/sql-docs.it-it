---
description: sp_replqueuemonitor (Transact-SQL)
title: sp_replqueuemonitor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replqueuemonitor
- sp_replqueuemonitor_TSQL
helpviewer_keywords:
- sp_replqueuemonitor
ms.assetid: 6909a3f1-43a2-4df5-a6a5-9e6f347ac841
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8fa6635b4f31692e5f45e848043e44496e2d702f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211667"
---
# <a name="sp_replqueuemonitor-transact-sql"></a>sp_replqueuemonitor (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Elenca i messaggi della coda di una [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] coda o [!INCLUDE[msCoName](../../includes/msconame-md.md)] di Accodamento messaggi per le sottoscrizioni ad aggiornamento in coda a una pubblicazione specificata. Se si utilizzano code di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la stored procedure viene eseguita nel database di sottoscrizione del Sottoscrittore. In caso di utilizzo di Message Queuing, la stored procedure viene eseguita nel database di distribuzione del server di distribuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_replqueuemonitor [ @publisher = ] 'publisher'  
    [ , [ @publisherdb = ] 'publisher_db' ]  
    [ , [ @publication = ] 'publication' ]  
    [ , [ @tranid = ] 'tranid' ]  
    [ , [ @queuetype = ] 'queuetype' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e il valore predefinito è null. Il server deve essere configurato per la pubblicazione. Il valore NULL indica tutti i server di pubblicazione.  
  
`[ @publisherdb = ] 'publisher_db' ]` Nome del database di pubblicazione. *publisher_db* è di **tipo sysname** e il valore predefinito è null. che indica tutti i database di pubblicazione.  
  
`[ @publication = ] 'publication' ]` Nome della pubblicazione. *Publication* è di **tipo sysname** e il valore predefinito è null. che indica tutte le pubblicazioni.  
  
`[ @tranid = ] 'tranid' ]` ID della transazione. *tranid* è di **tipo sysname** e il valore predefinito è null. che indica tutte le transazioni.  
  
`[ @queuetype = ] 'queuetype' ]` Tipo di coda in cui vengono archiviate le transazioni. *QueueType* è di **tinyint** e il valore predefinito è **0**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0**|Tutti i tipi di coda|  
|**1**|accodamento messaggi|  
|**2**|Coda di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_replqueuemonitor** viene utilizzata per la replica snapshot o transazionale con sottoscrizioni ad aggiornamento in coda. I messaggi in coda che non includono comandi SQL o che fanno parte di un comando SQL esteso non vengono visualizzati.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_replqueuemonitor**.  
  
## <a name="see-also"></a>Vedere anche  
 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
