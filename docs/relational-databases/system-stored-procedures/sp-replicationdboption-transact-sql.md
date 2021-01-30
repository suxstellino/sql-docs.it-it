---
description: sp_replicationdboption (Transact-SQL)
title: sp_replicationdboption (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replicationdboption_TSQL
- sp_replicationdboption
helpviewer_keywords:
- sp_replicationdboption
ms.assetid: d021864e-3f21-4d1a-89df-6c1086f753bf
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e56271516d9ef2df60e993cd73963d074b503d42
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211874"
---
# <a name="sp_replicationdboption-transact-sql"></a>sp_replicationdboption (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Imposta un'opzione del database di replica per il database specificato. Questa stored procedure viene eseguita in qualsiasi database del server di pubblicazione o del Sottoscrittore.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_replicationdboption [ @dbname= ] 'db_name'   
        , [ @optname= ] 'optname'   
        , [ @value= ] 'value'   
    [ , [ @ignore_distributor= ] ignore_distributor ]  
    [ , [ @from_scripting = ] from_scripting ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @dbname = ] 'dbname'` Database per cui è in corso l'impostazione dell'opzione del database di replica. *db_name* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @optname = ] 'optname'` Opzione del database di replica da abilitare o disabilitare. *optname* è di **tipo sysname**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**pubblicazione di tipo merge**|Specifica se il database può essere utilizzato per pubblicazioni di tipo merge.|  
|**pubblicare**|Specifica se il database può essere utilizzato per altri tipi di pubblicazione.|  
|**sottoscrivere**|Specifica se si tratta di un database di sottoscrizione.|  
|**sync with backup**|Specifica se il database è abilitato per il backup coordinato. Per ulteriori informazioni, vedere [abilitare backup coordinati per la replica transazionale &#40;&#41;di programmazione Transact-SQL della replica ](../../relational-databases/replication/administration/enable-coordinated-backups-for-transactional-replication.md).|  
  
`[ @value = ] 'value'` Indica se abilitare o disabilitare l'opzione del database di replica specificata. *value* è di **tipo sysname** e può essere **true** o **false**. Se questo valore è **false** e *optname* è **merge publish**, vengono eliminate anche le sottoscrizioni del database di pubblicazione di tipo merge.  
  
`[ @ignore_distributor = ] ignore_distributor` Indica se questo stored procedure viene eseguito senza connettersi al server di distribuzione. *ignore_distributor* è di **bit** e il valore predefinito è **0**, che indica che il server di distribuzione deve essere connesso e aggiornato con il nuovo stato del database di pubblicazione. Il valore **1** deve essere specificato solo se il server di distribuzione non è accessibile e **sp_replicationdboption** viene utilizzato per disabilitare la pubblicazione.  
  
`[ @from_scripting = ] from_scripting` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_replicationdboption** viene utilizzata per la replica snapshot, la replica transazionale e la replica di tipo merge.  
  
 Questa procedura crea o elimina tabelle del sistema di replica specifiche, account di sicurezza specifici e così via a seconda delle opzioni impostate. Imposta la **is_published** corrispondente (replica transacational o snapshot), **is_merge_published** (replica di tipo merge) o **is_distributor** bit nella tabella di sistema **master. databases** e crea le tabelle di sistema necessarie.  
  
 Per disabilitare la pubblicazione, è necessario che il database di pubblicazione sia online. Se esiste uno snapshot per il database di pubblicazione, deve essere eliminato prima della disabilitazione della pubblicazione. Uno snapshot del database è una copia offline e di sola lettura di un database e non è correlato a uno snapshot di replica. Per altre informazioni, vedere [Snapshot del database &#40;SQL Server&#41;](../../relational-databases/databases/database-snapshots-sql-server.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_replicationdboption**.  
  
## <a name="see-also"></a>Vedere anche  
 [Configurare la pubblicazione e la distribuzione](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Eliminare una pubblicazione](../../relational-databases/replication/publish/delete-a-publication.md)   
 [Disabilitare la pubblicazione e la distribuzione](../../relational-databases/replication/disable-publishing-and-distribution.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
