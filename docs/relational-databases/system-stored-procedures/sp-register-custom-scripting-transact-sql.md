---
description: sp_register_custom_scripting (Transact-SQL)
title: sp_register_custom_scripting (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_register_custom_scripting
- sp_register_custom_scripting_TSQL
helpviewer_keywords:
- sp_register_custom_scripting
ms.assetid: a8159282-de3b-4b9e-bdc9-3d3fce485c7f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f9239f89c5f65f0a4af3ddfd5588a92551dfbcdc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210449"
---
# <a name="sp_register_custom_scripting-transact-sql"></a>sp_register_custom_scripting (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La replica consente di sostituire una o più stored procedure predefinite utilizzate per la replica transazionale con stored procedure personalizzate definite dall'utente. Quando viene apportata una modifica dello schema a una tabella replicata, queste stored procedure vengono ricreate. **sp_register_custom_scripting** registra un stored procedure o un [!INCLUDE[tsql](../../includes/tsql-md.md)] file di script che viene eseguito quando si verifica una modifica dello schema per inserire nello script la definizione per un nuovo stored procedure personalizzato definito dall'utente. Questa nuova stored procedure personalizzata definita dall'utente deve riflettere il nuovo schema della tabella. **sp_register_custom_scripting** viene eseguita nel database di pubblicazione del server di pubblicazione e il file di script o stored procedure registrato viene eseguito nel Sottoscrittore quando si verifica una modifica dello schema.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_register_custom_scripting [ @type  = ] 'type'  
    [ @value = ] 'value'   
    [ , [ @publication = ] 'publication' ]  
    [ , [ @article = ] 'article' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @type = ] 'type'` Tipo di stored procedure o script personalizzato da registrare. il *tipo* è **varchar (16)** e non prevede alcun valore predefinito. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**insert**|La stored procedure personalizzata registrata viene eseguita quando viene replicata un'istruzione INSERT.|  
|**update**|La stored procedure personalizzata registrata viene eseguita quando viene replicata un'istruzione UPDATE.|  
|**delete**|La stored procedure personalizzata registrata viene eseguita quando viene replicata un'istruzione DELETE.|  
|**custom_script**|Lo script viene eseguito alla fine del trigger DDL (Data Definition Language).|  
  
`[ @value = ] 'value'` Nome di un stored procedure o nome e percorso completo del [!INCLUDE[tsql](../../includes/tsql-md.md)] file di script in fase di registrazione. *value* è di **tipo nvarchar (1024)** e non prevede alcun valore predefinito.  
  
> [!NOTE]  
>  Se si specifica NULL per il parametro *value*, verrà annullata la registrazione di uno script precedentemente registrato, che corrisponde all'esecuzione [sp_unregister_custom_scripting](../../relational-databases/system-stored-procedures/sp-unregister-custom-scripting-transact-sql.md).  
  
 Quando il valore di *tipo* è **custom_script**, è previsto il nome e il percorso completo di un [!INCLUDE[tsql](../../includes/tsql-md.md)] file di script. In caso contrario, il *valore* deve corrispondere al nome di un stored procedure registrato.  
  
`[ @publication = ] 'publication'` Nome della pubblicazione per cui viene registrato lo script o la stored procedure personalizzata. *Publication* è di **tipo sysname** e il valore predefinito è **null**.  
  
`[ @article = ] 'article'` Nome dell'articolo per cui viene registrato lo script o la stored procedure personalizzata. *article* è di **tipo sysname** e il valore predefinito è **null**.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_register_custom_scripting** viene utilizzata per la replica snapshot e transazionale.  
  
 Questa stored procedure deve essere eseguita prima di apportare una modifica dello schema a una tabella replicata. Per ulteriori informazioni sull'utilizzo di questa stored procedure, vedere [rigenerazione di procedure transazionali personalizzate per riflettere le modifiche dello schema](../../relational-databases/replication/transactional/transactional-articles-regenerate-to-reflect-schema-changes.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** , del ruolo predefinito del database **db_owner** o del ruolo predefinito del database **db_ddladmin** possono eseguire **sp_register_custom_scripting**.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_unregister_custom_scripting &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-unregister-custom-scripting-transact-sql.md)  
  
  
