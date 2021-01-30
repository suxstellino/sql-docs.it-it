---
description: sp_repldropcolumn (Transact-SQL)
title: sp_repldropcolumn (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_repldropcolumn_TSQL
- sp_repldropcolumn
helpviewer_keywords:
- sp_repldropcolumn
ms.assetid: fdc1ec5f-f108-42b4-a2d8-f06a71913ab8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6c5fdabf724c284e8a75244269321b92c81b46ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206424"
---
# <a name="sp_repldropcolumn-transact-sql"></a>sp_repldropcolumn (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Elimina una colonna da un articolo di tabella esistente pubblicato. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
> [!IMPORTANT]
>  Questa stored procedure è deprecata e viene supportata solo per motivi di compatibilità con le versioni precedenti. Deve essere utilizzato solo con [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] autori e [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] sottoscrittori di ripubblicazione. Questa stored procedure non deve essere utilizzata nelle colonne con tipi di dati introdotti in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] o versioni successive.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_repldropcolumn [ @source_object = ] 'source_object', [ @column = ] 'column'   
    [ , [ @from_agent = ] from_agent]   
    [ , [ @schema_change_script = ] 'schema_change_script' ]   
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]   
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]   
```  
  
## <a name="arguments"></a>Argomenti  
 [ @source_object =]'*source_object*'  
 Nome dell'articolo di tabella contenente la colonna da eliminare. *source_object* è di tipo nvarchar (258) e non prevede alcun valore predefinito.  
  
 [ @column =]'*colonna*'  
 Nome della colonna nella tabella da eliminare. *Column* è di tipo sysname e non prevede alcun valore predefinito.  
  
 [ @from_agent =] *from_agent*  
 Specifica se la stored procedure viene eseguita da un agente di replica. *from_agent* è di tipo int e il valore predefinito è 0, mentre il valore 1 viene utilizzato quando questo stored procedure viene eseguito da un agente di replica e in tutti gli altri casi è necessario utilizzare il valore predefinito 0.  
  
 [ @schema_change_script =]'*schema_change_script*'  
 Specifica il nome e il percorso di uno script di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzato per modificare le stored procedure personalizzate generate dal sistema. *schema_change_script* è di tipo nvarchar (4000) e il valore predefinito è null. La replica consente di sostituire una o più stored procedure predefinite utilizzate per la replica transazionale con stored procedure personalizzate definite dall'utente. *schema_change_script* viene eseguita dopo che è stata apportata una modifica dello schema a un articolo di tabella replicato utilizzando sp_repldropcolumn e che può essere utilizzato per eseguire una delle operazioni seguenti:  
  
-   Se le stored procedure personalizzate vengono rigenerate automaticamente, è possibile utilizzare *schema_change_script* per eliminare queste stored procedure personalizzate e sostituirle con stored procedure personalizzate definite dall'utente che supportano il nuovo schema.  
  
-   Se le stored procedure personalizzate non vengono rigenerate automaticamente, è possibile utilizzare *schema_change_script* per rigenerare tali stored procedure o per creare stored procedure personalizzate definite dall'utente.  
  
 [ @force_invalidate_snapshot =] *force_invalidate_snapshot*  
 Abilita o disabilita la funzionalità che consente di invalidare uno snapshot. *force_invalidate_snapshot* è di bit e il valore predefinito è 1.  
  
 Il valore 1 specifica che le modifiche all'articolo possono invalidare lo snapshot. In tal caso, il valore 1 consente l'esecuzione del nuovo snapshot.  
  
 Il valore 0 specifica che le modifiche apportate all'articolo non invalidano lo snapshot.  
  
 [ @force_reinit_subscription =] *force_reinit_subscription*  
 Abilita o disabilita la funzionalità che consente di reinizializzare la sottoscrizione. *force_reinit_subscription* è un bit e il valore predefinito è 0.  
  
 Il valore 0 specifica che le modifiche apportate all'articolo non causano la reinizializzazione della sottoscrizione.  
  
 Il valore 1 specifica che le modifiche apportate all'articolo possono causare la reinizializzazione della sottoscrizione. In tal caso, il valore 1 consente la reinizializzazione della sottoscrizione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server sysadmin sul server di pubblicazione o i membri dei ruoli predefiniti del database db_owner o db_ddladmin nel database di pubblicazione possono eseguire sp_repldropcolumn.  
  
## <a name="see-also"></a>Vedere anche  
 [Funzionalità deprecate in replica di SQL Server](../../relational-databases/replication/deprecated-features-in-sql-server-replication.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
