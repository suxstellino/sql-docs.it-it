---
description: sp_setreplfailovermode (Transact-SQL)
title: sp_setreplfailovermode (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_setreplfailovermode
- sp_setreplfailovermode_TSQL
helpviewer_keywords:
- sp_setreplfailovermode
ms.assetid: ca98a4c3-bea4-4130-88d7-79e0fd1e85f6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 216f76723d0c7d00461b5fd299bee01c87e99388
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176416"
---
# <a name="sp_setreplfailovermode-transact-sql"></a>sp_setreplfailovermode (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Consente di impostare la modalità di failover per le sottoscrizioni abilitate per l'aggiornamento immediato sostituito dall'aggiornamento in coda in caso di errore. Questa stored procedure viene eseguita nel database di sottoscrizione del Sottoscrittore. Per ulteriori informazioni sulle modalità di failover, vedere [sottoscrizioni aggiornabili per la replica transazionale](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_setreplfailovermode [ @publisher= ] 'publisher'  
    [ , [ @publisher_db = ] 'publisher_db' ]  
    [ , [ @publication= ] 'publication' ]  
    [ , [ @failover_mode= ] 'failover_mode' ]  
    [ , [ @override = ] override ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito. È necessario che la pubblicazione esista già.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database di pubblicazione. *publisher_db* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @failover_mode = ] 'failover_mode'` Modalità di failover per la sottoscrizione. *failover_mode* è di **tipo nvarchar (10)** . i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**immediate** o **Sync**|Per le modifiche apportate ai dati nel Sottoscrittore viene eseguita la copia bulk nel server di pubblicazione a mano a mano che vengono implementate.|  
|**accodati**|Le modifiche dei dati vengono archiviate in una [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] coda.|  
  
> [!NOTE]  
>  L'utilizzo di MSMQ ([!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queuing) è deprecato e non è più supportato.  
  
`[ @override = ] override` Solo per uso interno.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_setreplfailovermode** viene utilizzata per la replica snapshot o transazionale per le quali sono abilitate le sottoscrizioni, per l'aggiornamento in coda con failover all'aggiornamento immediato o per l'aggiornamento immediato con failover all'aggiornamento in coda.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_setreplfailovermode**.  
  
## <a name="see-also"></a>Vedere anche  
 [Passare da una modalità di aggiornamento all'altra per una sottoscrizione transazionale aggiornabile](../../relational-databases/replication/administration/switch-between-update-modes-for-an-updatable-transactional-subscription.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
