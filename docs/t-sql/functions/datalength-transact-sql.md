---
description: DATALENGTH (Transact-SQL)
title: DATALENGTH (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/20/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATALENGTH_TSQL
- DATALENGTH
dev_langs:
- TSQL
helpviewer_keywords:
- number of bytes representing expression
- data types [SQL Server], length
- DATALENGTH function
- expressions [SQL Server], length
- lengths [SQL Server], data
ms.assetid: 00f377f1-cc3e-4eac-be47-b3e3f80267c9
author: pmasl
ms.author: pelopes
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 93e028748976e1b382231c07c2df08ff6efb88d8
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754661"
---
# <a name="datalength-transact-sql"></a>DATALENGTH (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Questa funzione restituisce il numero di byte usati per rappresentare un'espressione.

> [!NOTE]
> Per restituire il numero di caratteri in un'espressione stringa, usare la funzione [LEN](../../t-sql/functions/len-transact-sql.md).
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DATALENGTH ( expression )   
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*expression*  
[Espressione](../../t-sql/language-elements/expressions-transact-sql.md) con qualsiasi tipo di dati.
  
## <a name="return-types"></a>Tipi restituiti
**bigint** se *expression* è del tipo di dati **nvarchar(max)**, **varbinary(max)** o **varchar(max)**. In caso contrario, **int**.
  
## <a name="remarks"></a>Osservazioni  
`DATALENGTH` risulta molto utile quando viene usata con tipi di dati che possono archiviare dati a lunghezza variabile, ad esempio:
- **image**
- **ntext**
- **nvarchar**
- **text**
- **varbinary**
- **varchar**
  
Per un valore NULL, `DATALENGTH` restituisce NULL.
  
> [!NOTE]  
> I livelli di compatibilità possono influire sui valori restituiti. Vedere [Livello di compatibilità ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md) per informazioni sui livelli di compatibilità supportati.  

> [!NOTE]
> Usare [LEN](../../t-sql/functions/len-transact-sql.md) per restituire il numero di caratteri codificati in una determinata espressione stringa e [DATALENGTH](../../t-sql/functions/datalength-transact-sql.md) per restituire la dimensione in byte per un'espressione stringa specificata. Questi output possono variare a seconda del tipo di dati e del tipo di codifica usati nella colonna. Per altre informazioni sulle differenze di archiviazione tra tipi di codifica diversi, vedere [Regole di confronto e supporto Unicode](../../relational-databases/collations/collation-and-unicode-support.md).

## <a name="examples"></a>Esempi  
Questo esempio trova la lunghezza della colonna `Name` nella tabella `Product`:
  
```sql
USE AdventureWorks2016  
GO
SELECT length = DATALENGTH(EnglishProductName), EnglishProductName  
FROM dbo.DimProduct  
ORDER BY EnglishProductName;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche
[LEN &#40;Transact-SQL&#41;](../../t-sql/functions/len-transact-sql.md)  
[CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[Funzioni di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)
