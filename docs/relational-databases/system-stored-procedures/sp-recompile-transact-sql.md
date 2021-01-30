---
description: sp_recompile (Transact-SQL)
title: sp_recompile (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_recompile_TSQL
- sp_recompile
dev_langs:
- TSQL
helpviewer_keywords:
- sp_recompile
ms.assetid: 6192ca87-febd-4075-8199-14b4fa609b8c
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a5ff0697d2ca09677455c700b1dfc45f16f1ef72
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200644"
---
# <a name="sp_recompile-transact-sql"></a>sp_recompile (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Causa la ricompilazione di stored procedure, trigger e funzioni definite dall'utente alla successiva esecuzione degli stessi. Ciò avviene tramite l'eliminazione del piano esistente dalla cache delle procedure e la creazione forzata di un nuovo piano alla successiva esecuzione della stored procedure o del trigger. In una raccolta [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], viene registrato l'evento SP:CacheInsert anziché l'evento SP:Recompile.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
  
sp_recompile [ @objname = ] 'object'  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @objname =]'*oggetto*'  
 Nome qualificato o non qualificato di una stored procedure, un trigger, una tabella, una vista o una funzione definita dall'utente nel database corrente. l' *oggetto* è di **tipo nvarchar (776)** e non prevede alcun valore predefinito. Se *Object* è il nome di un stored procedure, un trigger o una funzione definita dall'utente, il stored procedure, il trigger o la funzione verranno ricompilati alla successiva esecuzione. Se *Object* è il nome di una tabella o di una vista, tutte le stored procedure, i trigger o le funzioni definite dall'utente che fanno riferimento alla tabella o alla vista verranno ricompilati alla successiva esecuzione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (esito positivo) o un numero diverso da zero (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 La stored procedure sp_recompile esegue la ricerca di un oggetto solo nel database corrente.  
  
 Le query usate da stored procedure, trigger e funzioni definite dall'utente vengono ottimizzate solo in fase di compilazione. Poiché le indicizzazioni o le altre modifiche che hanno effetto sulle statistiche vengono effettuate nel database, il livello di efficienza di stored procedure, trigger e funzioni definite dall'utente dopo la compilazione potrebbe risultare minore. Tramite la ricompilazione delle stored procedure e dei trigger che modificano una tabella, è possibile riottimizzare le query.  
  
> [!NOTE]  
>  In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le stored procedure, i trigger e le funzioni definite dall'utente vengono ricompilati automaticamente quando la ricompilazione risulta un'operazione vantaggiosa.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione ALTER per l'oggetto specificato.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene definita la ricompilazione delle stored procedure, dei trigger e delle funzioni definite dall'utente che modificano la tabella `Customer` alla loro successiva esecuzione.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_recompile N'Sales.Customer';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
