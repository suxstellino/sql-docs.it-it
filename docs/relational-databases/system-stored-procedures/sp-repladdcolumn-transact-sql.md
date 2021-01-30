---
description: sp_repladdcolumn (Transact-SQL)
title: sp_repladdcolumn (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_repladdcolumn_TSQL
- sp_repladdcolumn
helpviewer_keywords:
- sp_repladdcolumn
ms.assetid: d6220f9f-c738-4f9c-bcf8-419994e86c81
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3579b68a229bd939be61bdb233e5d429dd0ca4c7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193542"
---
# <a name="sp_repladdcolumn-transact-sql"></a>sp_repladdcolumn (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Aggiunge una colonna a un articolo di tabella esistente che è stato pubblicato. È possibile aggiungere la nuova colonna in tutti i server di pubblicazione che pubblicano la tabella specificata oppure solo in una pubblicazione specifica della tabella. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
> [!IMPORTANT]
>  Questa stored procedure è deprecata e viene supportata per motivi di compatibilità con le versioni precedenti. Deve essere utilizzato solo con [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] autori e [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] sottoscrittori di ripubblicazione. Questa stored procedure non deve essere utilizzata nelle colonne con tipi di dati introdotti in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] o versioni successive.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_repladdcolumn [ @source_object = ] 'source_object', [ @column = ] 'column' ]  
    [ , [ @typetext = ] 'typetext' ]  
    [ , [ @publication_to_add = ] 'publication_to_add' ]  
    [ , [ @from_agent = ] from_agent ]  
    [ , [ @schema_change_script = ] 'schema_change_script' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @source_object =]'*source_object*'  
 Nome dell'articolo di tabella che contiene la nuova colonna da aggiungere. *source_object* è di **tipo nvarchar (358**) e non prevede alcun valore predefinito.  
  
 [ @column =]'*colonna*'  
 Nome della colonna della tabella da aggiungere per la replica. *Column* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
 [ @typetext = ]'*TypeText*'  
 Definizione della colonna da aggiungere. *TypeText* è di **tipo nvarchar (3000)** e non prevede alcun valore predefinito. Se, ad esempio, la colonna order_filled viene aggiunta ed è un campo a carattere singolo, non NULL, e il valore predefinito è **N**, order_filled sarebbe il parametro della *colonna* , mentre la definizione del **vincolo char (1) not null constraint_name valore predefinito ' n'** corrisponderebbe al parametro *TypeText* .  
  
 [ @publication_to_add =]'*publication_to_add*'  
 Nome della pubblicazione cui si desidera aggiungere la nuova colonna. *publication_to_add* è di **tipo nvarchar (4000)** e il valore predefinito è **All**. Se **tutte** le pubblicazioni contenenti questa tabella sono interessate. Se viene specificato *publication_to_add* , viene aggiunta la nuova colonna solo per questa pubblicazione.  
  
 [ @from_agent =] *from_agent*  
 Specifica se la stored procedure viene eseguita da un agente di replica. *from_agent* è di **tipo int** e il valore predefinito è **0**, mentre il valore **1** viene utilizzato quando questo stored procedure viene eseguito da un agente di replica e in tutti gli altri casi è necessario utilizzare il valore predefinito **0**.  
  
 [ @schema_change_script =]'*schema_change_script*'  
 Specifica il nome e il percorso di uno script di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzato per modificare le stored procedure personalizzate generate dal sistema. *schema_change_script* è di **tipo nvarchar (4000)** e il valore predefinito è null. La replica consente di sostituire una o più stored procedure predefinite utilizzate per la replica transazionale con stored procedure personalizzate definite dall'utente. *schema_change_script* viene eseguita dopo che è stata apportata una modifica dello schema a un articolo di tabella replicato utilizzando sp_repladdcolumn e che può essere utilizzato per eseguire una delle operazioni seguenti:  
  
-   Se le stored procedure personalizzate vengono rigenerate automaticamente, è possibile utilizzare *schema_change_script* per eliminare queste stored procedure personalizzate e sostituirle con stored procedure personalizzate definite dall'utente che supportano il nuovo schema.  
  
-   Se le stored procedure personalizzate non vengono rigenerate automaticamente, è possibile utilizzare *schema_change_script* per rigenerare tali stored procedure o per creare stored procedure personalizzate definite dall'utente.  
  
 [ @force_invalidate_snapshot =] *force_invalidate_snapshot*  
 Abilita o disabilita la funzionalità che consente di invalidare uno snapshot. *force_invalidate_snapshot* è di **bit** e il valore predefinito è **1**.  
  
 **1** specifica che le modifiche apportate all'articolo possono causare l'invalidità dello snapshot. in tal caso, il valore **1** consente di eseguire il nuovo snapshot.  
  
 **0** specifica che le modifiche apportate all'articolo non invalidano lo snapshot.  
  
 [ @force_reinit_subscription =] *force_reinit_subscription*  
 Abilita o disabilita la funzionalità che consente di reinizializzare la sottoscrizione. *force_reinit_subscription* è un **bit** e il valore predefinito è **0**.  
  
 **0** specifica che le modifiche apportate all'articolo non provocano la reinizializzazione della sottoscrizione.  
  
 **1** specifica che le modifiche apportate all'articolo possono causare la reinizializzazione della sottoscrizione. in tal caso, il valore **1** consente la reinizializzazione della sottoscrizione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server sysadmin e del ruolo predefinito del database db_owner possono eseguire sp_repladdcolumn.  
  
## <a name="see-also"></a>Vedere anche  
 [Funzionalità deprecate in replica di SQL Server](../../relational-databases/replication/deprecated-features-in-sql-server-replication.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
