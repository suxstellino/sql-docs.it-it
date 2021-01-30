---
description: _ (carattere jolly per corrispondenze di singoli caratteri) (Transact-SQL)
title: _ (Carattere jolly per corrispondenze di singoli caratteri) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/06/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Match
- wildcard
- _TSQL
- Match One
- _
dev_langs:
- TSQL
helpviewer_keywords:
- wildcard characters [SQL Server]
- _ (wildcard - match one character)
ms.assetid: 11a2ed36-9e21-4bdf-ae20-a31db1434b97
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9c82aed5ad9ae3fa43ba17825a8682be40b2aabe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202104"
---
# <a name="_-wildcard---match-one-character-transact-sql"></a>_ (carattere jolly per corrispondenze di singoli caratteri) (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Usare il carattere di sottolineatura _ per individuare singoli caratteri in un'operazione di confronto stringhe con criteri di ricerca, ad esempio `LIKE` e `PATINDEX`.  
  
## <a name="examples"></a>Esempi  

## <a name="a-simple-example"></a>A. Esempio semplice   

L'esempio seguente restituisce tutti i nomi di database che iniziano con la lettera `m` e la cui terza lettera è la lettera `d`. Il carattere di sottolineatura specifica che il secondo carattere del nome può essere qualsiasi lettera. I database `model` e `msdb` soddisfano questi criteri. Il database `master` non li soddisfa.

```sql
SELECT name FROM sys.databases
WHERE name LIKE 'm_d%';
```   
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]   
```
name
-----
model
msdb
```   
È possibile che altri database soddisfino questi criteri.

È possibile usare più caratteri di sottolineatura per rappresentare più caratteri. La modifica del criterio `LIKE` in modo da includere due caratteri di sottolineatura `'m__%` determinerà l'inclusione del database master nel risultato.

### <a name="b-more-complex-example"></a>B. Esempio più complesso
 Nell'esempio seguente viene usato l'operatore _ per trovare tutte le persone nella tabella `Person` con un nome composto da tre lettere che termina con `an`.  
  
```sql  
-- USE AdventureWorks2012
  
SELECT FirstName, LastName  
FROM Person.Person  
WHERE FirstName LIKE '_an'  
ORDER BY FirstName;  
```  
## <a name="c-escaping-the-underscore-character"></a>C: Escape del carattere di sottolineatura   
L'esempio seguente restituisce i nomi dei ruoli predefiniti del database come `db_owner` e `db_ddladmin`, ma restituisce anche l'utente `dbo`. 

```sql
SELECT name FROM sys.database_principals
WHERE name LIKE 'db_%';
```

Il carattere di sottolineatura nella posizione di terzo carattere viene considerato come un carattere jolly e non viene applicato alcun filtro per restituire solo le entità che iniziano con le lettere `db_`. Racchiudere il carattere di sottolineatura tra parentesi quadre `[_]`. 

```sql
SELECT name FROM sys.database_principals
WHERE name LIKE 'db[_]%';
```   
Ora l'utente `dbo` è escluso.   
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]   
```
name
-------------
db_owner
db_accessadmin
db_securityadmin
...
```

  
## <a name="see-also"></a>Vedere anche  
 [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)   
 [PATINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/patindex-transact-sql.md)   
  [% (Caratteri jolly per la corrispondenza)](../../t-sql/language-elements/percent-character-wildcard-character-s-to-match-transact-sql.md)   
  [&#91; &#93; (carattere jolly)](../../t-sql/language-elements/wildcard-character-s-to-match-transact-sql.md)   
 [&#91;^&#93; (caratteri jolly per la mancata corrispondenza dei caratteri)](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)     
  
