---
description: IDENT_SEED (Transact-SQL)
title: IDENT_SEED (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IDENT_SEED_TSQL
- IDENT_SEED
dev_langs:
- TSQL
helpviewer_keywords:
- identity columns [SQL Server], IDENT_SEED function
- seed values [SQL Server]
- IDENT_SEED function
ms.assetid: e4cb8eb8-affb-4810-a8a9-0110af3c247a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: cfaf7ca485c1b7b4bb047b66cc41cd2978dda1d1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197604"
---
# <a name="ident_seed-transact-sql"></a>IDENT_SEED (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce il valore di inizializzazione originale specificato durante la creazione di una colonna Identity in una tabella o una vista. La modifica del valore corrente di una colonna Identity usando DBCC CHECKIDENT non comporta anche la modifica del valore restituito da questa funzione.  
  
 ![Icona di collegamento a un articolo](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un articolo") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
IDENT_SEED ( 'table_or_view' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 **'** *table_or_view* **'**  
 [Espressione](../../t-sql/language-elements/expressions-transact-sql.md) che specifica la tabella o la vista in cui verificare la presenza di un valore di inizializzazione Identity. *table_or_view* può essere una costante stringa di caratteri racchiusa tra virgolette, una variabile, una funzione o un nome di colonna. *table_or_view* è di tipo **char**, **nchar**, **varchar** o **nvarchar**.  
  
## <a name="return-types"></a>Tipi restituiti  
**numerico**([@@MAXPRECISION](../../t-sql/functions/max-precision-transact-sql.md),0))  
  
## <a name="exceptions"></a>Eccezioni  
 Restituisce NULL in caso di errore o se un chiamante non ha l'autorizzazione necessaria per visualizzare l'oggetto.  
  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] un utente può visualizzare solo i metadati delle entità a protezione diretta di cui è proprietario o per cui ha ricevuto un'autorizzazione. Le funzioni predefinite di creazione dei metadati, ad esempio IDENT_SEED, non potranno quindi restituire NULL se l'utente non ha autorizzazioni per l'oggetto. Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-returning-the-seed-value-from-a-specified-table"></a>R. Restituzione del valore di inizializzazione da una tabella specificata  
 Nell'esempio seguente viene restituito il valore di inizializzazione per la tabella `Person.Address` del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT IDENT_SEED('Person.Address') AS Identity_Seed;  
GO  
```  
  
### <a name="b-returning-the-seed-value-from-multiple-tables"></a>B. Restituzione del valore di inizializzazione da più tabelle  
 L'esempio seguente restituisce le tabelle del database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] che includono una colonna Identity con un valore di inizializzazione.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT TABLE_SCHEMA, TABLE_NAME,   
   IDENT_SEED(TABLE_SCHEMA + '.' + TABLE_NAME) AS IDENT_SEED  
FROM INFORMATION_SCHEMA.TABLES  
WHERE IDENT_SEED(TABLE_SCHEMA + '.' + TABLE_NAME) IS NOT NULL;  
GO  
```  
  
 Set di risultati parziale:  
  
 ```
 TABLE_SCHEMA       TABLE_NAME                   IDENT_SEED  
------------       ---------------------------  -----------  
Person             Address                                1  
Production         ProductReview                          1  
Production         TransactionHistory                100000  
Person             AddressType                            1  
Production         ProductSubcategory                     1  
Person             vAdditionalContactInfo                 1  
dbo                AWBuildVersion                         1
```  
  
## <a name="see-also"></a>Vedere anche  
 [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Funzioni di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [IDENT_CURRENT &#40;Transact-SQL&#41;](../../t-sql/functions/ident-current-transact-sql.md)   
 [IDENT_INCR &#40;Transact-SQL&#41;](../../t-sql/functions/ident-incr-transact-sql.md)   
 [DBCC CHECKIDENT &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md)   
 [sys.identity_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-identity-columns-transact-sql.md)  
  
