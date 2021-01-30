---
description: sp_adddistributor (Transact-SQL)
title: sp_adddistributor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_adddistributor
- sp_adddistributor_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_adddistributor
ms.assetid: 35415502-68d0-40f6-993c-180e50004f1e
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: 2e84824fec7a45fcfea24a2d97ced622b2147f34
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182800"
---
# <a name="sp_adddistributor-transact-sql"></a>sp_adddistributor (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea una voce nella tabella [ serversys.sys](../../relational-databases/system-compatibility-views/sys-sysservers-transact-sql.md) (se non ne è presente una), contrassegna la voce del server come server di distribuzione e archivia le informazioni sulle proprietà. Questa stored procedure viene eseguita nel database master del server di distribuzione per registrare e contrassegnare il server come server di distribuzione. Nel caso di un server di distribuzione remoto la stored procedure viene eseguita nel database master del server di pubblicazione per registrare il server di distribuzione remoto.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_adddistributor [ @distributor= ] 'distributor'   
    [ , [ @heartbeat_interval= ] heartbeat_interval ]   
    [ , [ @password= ] 'password' ]   
    [ , [ @from_scripting= ] from_scripting ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @distributor = ] 'distributor'` Nome del server di distribuzione. *Distributor* è di **tipo sysname** e non prevede alcun valore predefinito. Questo parametro viene utilizzato solo per la configurazione di un server di distribuzione remoto. Vengono aggiunte voci per le proprietà del server di distribuzione nel **database msdb. Tabella MSdistributor** .  

> [!NOTE]
> Il nome del server può essere specificato come `<Hostname>,<PortNumber>` . Potrebbe essere necessario specificare il numero di porta per la connessione quando SQL Server viene distribuito in Linux o Windows con una porta personalizzata e il servizio browser è disabilitato.

`[ @heartbeat_interval = ] heartbeat_interval` Numero massimo di minuti per cui un agente può passare senza registrare un messaggio di stato. *heartbeat_interval* è di **tipo int** e il valore predefinito è 10 minuti. Viene creato un processo di SQL Server Agent che viene eseguito in base a questo intervallo per controllare lo stato degli agenti di replica in esecuzione.  
  
`[ @password = ] 'password']` Password dell'account di accesso **distributor_admin** . *password* è di **tipo sysname** e il valore predefinito è null. Se il valore è NULL o una stringa vuota, la password viene reimpostata su un valore casuale. La password deve essere configurata quando viene aggiunto il primo server di distribuzione remoto. **distributor_admin** account di accesso e la *password* vengono archiviati per la voce del server collegato utilizzata per una connessione RPC del database di *distribuzione* , incluse le connessioni locali. Se il *server di distribuzione* è locale, la password per **distributor_admin** è impostata su un nuovo valore. Per i server di pubblicazione con un server di distribuzione remoto, è necessario specificare lo stesso valore di *password* durante l'esecuzione di **Sp_adddistributor** sia nel server di pubblicazione che nel server di distribuzione. per modificare la password del server di distribuzione è possibile utilizzare [sp_changedistributor_password](../../relational-databases/system-stored-procedures/sp-changedistributor-password-transact-sql.md) .  
  
> [!IMPORTANT]  
>  Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali in un file script, è fondamentale proteggere il file per evitare accessi non autorizzati.  
  
`[ @from_scripting = ] from_scripting` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 **sp_adddistributor** viene utilizzata per la replica snapshot, la replica transazionale e la replica di tipo merge.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#AddDistPub](../../relational-databases/replication/codesnippet/tsql/sp-adddistributor-transa_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_adddistributor**.  
  
## <a name="see-also"></a>Vedere anche  
 [Configurare la pubblicazione e la distribuzione](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [sp_changedistributor_property &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changedistributor-property-transact-sql.md)   
 [sp_dropdistributor &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropdistributor-transact-sql.md)   
 [sp_helpdistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Configurare la distribuzione](../../relational-databases/replication/configure-distribution.md)  
  
  
