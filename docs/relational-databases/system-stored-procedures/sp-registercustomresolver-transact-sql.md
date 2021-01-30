---
description: sp_registercustomresolver (Transact-SQL)
title: sp_registercustomresolver (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_registercustomresolver
- sp_registercustomresolver_TSQL
helpviewer_keywords:
- sp_registercustomresolver
ms.assetid: 6d2b0472-0e1f-4005-833c-735d1940fe93
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4a4c08e8643aedaecd415177ef87f9fadc69563f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185691"
---
# <a name="sp_registercustomresolver-transact-sql"></a>sp_registercustomresolver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Registra un gestore della logica di business o un sistema di risoluzione personalizzato basato su COM che può essere richiamato durante il processo di sincronizzazione della replica di tipo merge. Questa stored procedure viene eseguita nel database di distribuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_registercustomresolver [ @article_resolver = ] 'article_resolver'   
    [ , [ @resolver_clsid = ] 'resolver_clsid' ]  
    [ , [ @is_dotnet_assembly = ] 'is_dotnet_assembly' ]  
    [ , [ @dotnet_assembly_name = ] 'dotnet_assembly_name' ]  
    [ , [ @dotnet_class_name = ] 'dotnet_class_name' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @article_resolver = ] 'article_resolver'` Specifica il nome descrittivo per la logica di business personalizzata in fase di registrazione. *article_resolver* è di **tipo nvarchar (255)** e non prevede alcun valore predefinito.  
  
`[ @resolver_clsid = ] 'resolver_clsid'` Specifica il valore CLSID dell'oggetto COM da registrare. *Resolver_clsid* della logica di business personalizzata è di **tipo nvarchar (50)** e il valore predefinito è null. È necessario impostare questo parametro su un valore CLSID valido oppure su NULL in caso di registrazione di un assembly di un gestore della logica di business.  
  
`[ @is_dotnet_assembly = ] 'is_dotnet_assembly'` Specifica il tipo di logica di business personalizzata in fase di registrazione. *is_dotnet_assembly* è di **tipo nvarchar (50)** e il valore predefinito è false. **true** indica che la logica di business personalizzata in fase di registrazione è un assembly del gestore della logica di business. **false** indica che si tratta di un componente com.  
  
`[ @dotnet_assembly_name = ] 'dotnet_assembly_name'` Nome dell'assembly che implementa il gestore della logica di business. *dotnet_assembly_name* è di **tipo nvarchar (255)** e il valore predefinito è null. È necessario specificare il percorso completo dell'assembly se non viene distribuito nella stessa directory dell'eseguibile dell'agente di merge, nella stessa directory dell'applicazione che avvia l'agente di merge in modalità sincrona oppure nella Global Assembly Cache (GAC).  
  
`[ @dotnet_class_name = ] 'dotnet_class_name'` Nome della classe che esegue l'override <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> di per implementare il gestore della logica di business. Il nome deve essere specificato nel formato **Namespace. NomeClasse**. *dotnet_class_name* è di **tipo nvarchar (255)** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_registercustomresolver** viene utilizzata nella replica di tipo merge.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_registercustomresolver**.  
  
## <a name="see-also"></a>Vedere anche  
 [Implementare un gestore della logica di business per un articolo di merge](../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md)   
 [Implementare un sistema di risoluzione dei conflitti personalizzato per un articolo di merge](../../relational-databases/replication/implement-a-custom-conflict-resolver-for-a-merge-article.md)   
 [sp_lookupcustomresolver &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-lookupcustomresolver-transact-sql.md)   
 [sp_unregistercustomresolver &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-unregistercustomresolver-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
