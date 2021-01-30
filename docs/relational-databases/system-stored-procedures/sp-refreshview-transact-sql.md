---
description: sp_refreshview (Transact-SQL)
title: sp_refreshview (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_refreshview
- sp_refreshview_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_refreshview
ms.assetid: 9ce1d07c-ee66-4a83-8c73-cd2cc104dd08
author: markingmyname
ms.author: maghan
ms.openlocfilehash: da179448924381de2dddbf5613194c30df1f01bd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201265"
---
# <a name="sp_refreshview-transact-sql"></a>sp_refreshview (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Aggiorna i metadati per la vista non associata a schema specificata. I metadati persistenti di una vista possono diventare obsoleti in seguito alla modifica degli oggetti sottostanti su cui è basata la vista.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_refreshview [ @viewname = ] 'viewname'   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @viewname = ] 'viewname'` Nome della vista. *viewName* è di **tipo nvarchar** e non prevede alcun valore predefinito. *viewName* può essere un identificatore multipart, ma può fare riferimento solo alle viste nel database corrente.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (esito positivo) o un numero diverso da zero (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 Se una vista non viene creata con l'oggetto schema, è necessario eseguire **sp_refreshview** quando vengono apportate modifiche agli oggetti sottostanti la vista che influiscono sulla definizione della vista. In caso contrario, le query sulla vista possono generare risultati imprevisti.  
  
## <a name="permissions"></a>Autorizzazioni  
 Sono richieste l'autorizzazione ALTER per la vista e l'autorizzazione REFERENCES per i tipi CLR (Common Language Runtime) definiti dall'utente e le raccolte di XML Schema a cui fanno riferimento le colonne della vista.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-updating-the-metadata-of-a-view"></a>R. Aggiornamento dei metadati di una vista  
 Nell'esempio seguente vengono aggiornati i metadati della vista `Sales.vIndividualCustomer`.  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sp_refreshview N'Sales.vIndividualCustomer';  
```  
  
### <a name="b-creating-a-script-that-updates-all-views-that-have-dependencies-on-a-changed-object"></a>B. Creazione di uno script con cui vengono aggiornate tutte le viste con dipendenze da un oggetto modificato  
 Si supponga che la tabella `Person.Person` sia stata modificata in modo da influire sulla definizione di qualsiasi vista creata in base a essa. Nell'esempio seguente viene creato uno script con cui vengono aggiornati i metadati di tutte le viste con una dipendenza dalla tabella `Person.Person`.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT DISTINCT 'EXEC sp_refreshview ''' + name + ''''   
FROM sys.objects AS so   
INNER JOIN sys.sql_expression_dependencies AS sed   
    ON so.object_id = sed.referencing_id   
WHERE so.type = 'V' AND sed.referenced_id = OBJECT_ID('Person.Person');  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)   
 [sp_refreshsqlmodule &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-refreshsqlmodule-transact-sql.md)  
  
  
