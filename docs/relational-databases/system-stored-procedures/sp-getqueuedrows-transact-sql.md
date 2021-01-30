---
description: sp_getqueuedrows (Transact-SQL)
title: sp_getqueuedrows (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_getqueuedrows_TSQL
- sp_getqueuedrows
helpviewer_keywords:
- sp_getqueuedrows
ms.assetid: 139e834f-1988-4b4d-ac81-db1f89ea90e8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ad904065a8270266e7010ff49889d04a8625296f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183143"
---
# <a name="sp_getqueuedrows-transact-sql"></a>sp_getqueuedrows (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Recupera le righe nel Sottoscrittore per le quali esistono aggiornamenti in sospeso nella coda. Questa stored procedure viene eseguita nel database di sottoscrizione del Sottoscrittore.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_getqueuedrows [ @tablename = ] 'tablename'  
    [ , [ @owner = ] 'owner'  
    [ , [ @tranid = ] 'transaction_id' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @tablename = ] 'tablename'` Nome della tabella. *TableName* è di **tipo sysname** e non prevede alcun valore predefinito. La tabella deve essere inclusa in una sottoscrizione in coda.  
  
`[ @owner = ] 'owner'` Proprietario della sottoscrizione. *owner* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @tranid = ] 'transaction_id'` Consente di filtrare l'output in base all'ID della transazione. *transaction_id* è di **tipo nvarchar (70)** e il valore predefinito è null. Se specificato, viene visualizzato l'ID della transazione associato al comando in coda. Se è NULL, vengono visualizzati tutti i comandi nella coda.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 Visualizza tutte le righe alle quali è associata almeno una transazione in coda per la tabella sottoscritta.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**Azione**|**nvarchar (10)**|Tipo di azione da eseguire in corrispondenza della sincronizzazione.<br /><br /> INS= inserimento<br /><br /> DEL = eliminazione<br /><br /> UPD = aggiornamento|  
|**Tranid**|**nvarchar (70)**|ID della transazione in cui è stato eseguito il comando.|  
|**table column1...n**||Valore per ogni colonna della tabella specificata in *TableName*.|  
|**msrepl_tran_version**|**uniqueidentifier**|Questa colonna viene utilizzata per tenere traccia delle modifiche ai dati replicati e per eseguire il rilevamento dei conflitti nel server di pubblicazione. La colonna viene aggiunta alla tabella automaticamente.|  
  
## <a name="remarks"></a>Commenti  
 **sp_getqueuedrows** viene utilizzato nei Sottoscrittori che partecipano all'aggiornamento in coda.  
  
 **sp_getqueuedrows** individua le righe di una tabella specificata in un database di sottoscrizione che hanno partecipato a un aggiornamento in coda, ma attualmente non sono state risolte dall'agente di lettura coda.  
  
## <a name="permissions"></a>Autorizzazioni  
 **sp_getqueuedrows** richiede autorizzazioni SELECT per la tabella specificata in *TableName*.  
  
## <a name="see-also"></a>Vedere anche  
 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [Rilevamento e risoluzione dei conflitti di aggiornamento in coda](../../relational-databases/replication/transactional/updatable-subscriptions-queued-updating-conflict-resolution.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
