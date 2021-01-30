---
description: '&#x40;&#x40;DBTS (Transact-SQL)'
title: DBTS (Transact-SQL)
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@DBTS_TSQL'
- '@@DBTS'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@DBTS function'
- timestamp data type
ms.assetid: 91842ddd-91c0-4445-a03f-116f6bc991d0
author: cawrites
ms.author: chadam
ms.openlocfilehash: 680c1486b4a9bf5b97d2b274af18eb82246b6619
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211712"
---
# <a name="x40x40dbts-transact-sql"></a>&#x40;&#x40;DBTS (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Questa funzione restituisce il valore del tipo di dati **timestamp** corrente per il database corrente. Il database corrente avrà un valore di timestamp univoco garantito.
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
@@DBTS  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
**varbinary**
  
## <a name="remarks"></a>Osservazioni  
@@DBTS restituisce l'ultimo valore timestamp del database corrente. Quando si inserisce o si aggiorna una riga che include una colonna **timestamp** viene generato un nuovo valore timestamp.
  
Le modifiche apportate ai livelli di isolamento delle transazioni non interessano la funzione @@DBTS.
  
## <a name="examples"></a>Esempi  
Questo esempio restituisce il **timestamp** corrente dal database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].
  
```sql
USE AdventureWorks2012;  
GO  
SELECT @@DBTS;  
```  
  
## <a name="see-also"></a>Vedere anche
[Funzioni di configurazione &#40;Transact-SQL&#41;](../../t-sql/functions/configuration-functions-transact-sql.md)  
[Concorrenza dei cursori &#40;ODBC&#41;](../../relational-databases/native-client-odbc-cursors/properties/cursor-concurrency-odbc.md)  
[Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[MIN_ACTIVE_ROWVERSION &#40;Transact-SQL&#41;](../../t-sql/functions/min-active-rowversion-transact-sql.md)
  
  
