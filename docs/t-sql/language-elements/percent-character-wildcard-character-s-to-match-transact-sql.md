---
description: Carattere di percentuale (carattere jolly) (Transact-SQL)
title: Ricerca con caratteri jolly (%)
ms.custom: seo-lt-2019
ms.date: 12/06/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '%'
- '%_TSQL'
- wildcard
dev_langs:
- TSQL
helpviewer_keywords:
- conditions [SQL Server], wildcards
- '% (wildcard - character(s) to match)'
- matching conditions [SQL Server]
- wildcard characters [SQL Server]
- characters [SQL Server], wildcard
ms.assetid: d4cbc1a9-37e1-4101-97fb-e6ac30c1223e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 27156d5bfcb6fe1e859131a6a011bba2b53b46ba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185287"
---
# <a name="percent-character-wildcard---characters-to-match-transact-sql"></a>Carattere di percentuale (carattere jolly) (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Corrisponde a qualsiasi stringa composta da zero o più caratteri. Questo carattere jolly può essere utilizzato come prefisso o suffisso.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente vengono restituiti tutti i nomi delle persone elencate nella tabella `Person` del database `AdventureWorks2012` che iniziano con `Dan`.  
  
```syntaxsql  
-- Uses AdventureWorks  
  
SELECT FirstName, LastName  
FROM Person.Person  
WHERE FirstName LIKE 'Dan%';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)   
 [Operatori &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Espressioni &#40; Transact-SQL &#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
 [&#91; &#93; (carattere jolly)](../../t-sql/language-elements/wildcard-character-s-to-match-transact-sql.md)   
  [&#91;^&#93; (carattere jolly per la mancata corrispondenza)](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)     
 [_ (carattere jolly per corrispondenze di singoli caratteri)](../../t-sql/language-elements/wildcard-match-one-character-transact-sql.md)  
    
  
