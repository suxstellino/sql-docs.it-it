---
description: sp_helpreplfailovermode (Transact-SQL)
title: sp_helpreplfailovermode (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpreplfailovermode
- sp_helpreplfailovermode_TSQL
helpviewer_keywords:
- sp_helpreplfailovermode
ms.assetid: d1090e42-6840-4bf6-9aa9-327fd8987ec2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 11ebe0df55bb0b6b5a2b7f009a0be4be28c788c1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210834"
---
# <a name="sp_helpreplfailovermode-transact-sql"></a>sp_helpreplfailovermode (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Visualizza la modalità di failover corrente di una sottoscrizione. Questa stored procedure viene eseguita in qualsiasi database del Sottoscrittore. Per ulteriori informazioni sulle modalità di failover, vedere [sottoscrizioni aggiornabili per la replica transazionale](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpreplfailovermode [ @publisher= ] 'publisher'   
    [ , [ @publisher_db = ] 'publisher_db' ]   
    [ , [ @publication = ] 'publication' ]   
    [ , [ @failover_mode_id= ] 'failover_mode_id'OUTPUT]   
    [ , [ @failover_mode = ] 'failover_mode'OUTPUT]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione che partecipa all'aggiornamento del Sottoscrittore. *Publisher* è di **tipo sysname** e non prevede alcun valore predefinito. Il server di pubblicazione deve essere già configurato per la pubblicazione.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database di pubblicazione. *publisher_db* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publication = ] 'publication'` Nome della pubblicazione che partecipa all'aggiornamento del Sottoscrittore. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @failover_mode_id = ] 'failover_mode_id' OUTPUT` Restituisce il valore intero della modalità di failover ed è un parametro di **output** . *failover_mode_id* è di un **tinyint** e il valore predefinito è **0**. Restituisce **0** per l'aggiornamento immediato e **1** per l'aggiornamento in coda.  
  
`[ @failover_mode = ] 'failover_mode' OUTPUT` Restituisce la modalità in cui vengono apportate modifiche ai dati nel Sottoscrittore. *failover_mode* è di **tipo nvarchar (10)** e il valore predefinito è null. È un parametro di **output** .  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**immediato**|Aggiornamento immediato: gli aggiornamenti implementati nel Sottoscrittore vengono propagati immediatamente al server di pubblicazione tramite il protocollo di commit in due fasi (2PC).|  
|**accodati**|Aggiornamento in coda: gli aggiornamenti implementati nel Sottoscrittore vengono archiviati in una coda.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_helpreplfailovermode** viene utilizzata per la replica snapshot o transazionale per le quali le sottoscrizioni sono abilitate per l'aggiornamento immediato con aggiornamento in coda come failover, in caso di errore.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_helpreplfailovermode**.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_setreplfailovermode &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-setreplfailovermode-transact-sql.md)  
  
  
