---
description: PARSENAME (Transact-SQL)
title: PARSENAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/02/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PARSENAME_TSQL
- PARSENAME
dev_langs:
- TSQL
helpviewer_keywords:
- PARSENAME function
- parsing [SQL Server], PARSENAME function
- names [SQL Server], objects
- objects [SQL Server], names
- part of object names [SQL Server]
ms.assetid: abf34f99-9ee9-460b-85b2-930ca5c4b5ae
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 349c0a655e7f4038eb74a1123b0ff7c9201ba2d8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99101725"
---
# <a name="parsename-transact-sql"></a>PARSENAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce la parte specificata del nome di un oggetto. Le parti di un oggetto che è possibile recuperare sono il nome dell'oggetto, il nome dello schema, il nome del database e il nome del server. 
  
> [!NOTE]  
>  La funzione PARSENAME non indica se esiste un oggetto avente il nome specificato. PARSENAME restituisce solo la parte specificata del nome di oggetto specificato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql 
PARSENAME ('object_name' , object_piece )
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

*'object_name'* è il parametro che contiene il nome dell'oggetto per cui recuperare la parte specificata. Questo parametro rappresenta un nome di oggetto che può essere qualificato facoltativamente. Se vengono qualificate tutte le parti del nome dell'oggetto, il nome può essere costituito da quattro parti, ovvero nome del server, nome del database, nome dello schema e nome dell'oggetto.  Ogni parte della stringa 'object_name' è di tipo *sysname* che equivale a nvarchar(128) o a 256 byte. Se una parte della stringa supera i 256 byte, PARSENAME restituirà NULL per tale parte perché non è un valore sysname valido.
  
*object_piece*  
Parte dell'oggetto da restituire. *object_piece* è di tipo **int**. I possibili valori sono i seguenti:  
    1 = nome dell'oggetto  
    2 = nome dello schema  
    3 = nome del database  
    4 = nome del server  
  
## <a name="return-type"></a>Tipo restituito

 **sysname**
  
## <a name="remarks"></a>Osservazioni

 PARSENAME restituisce NULL se si verifica una delle condizioni seguenti:  
  
-   *object_name* o *object_piece* è NULL.  
  
-   Si verifica un errore di sintassi.  
  
 La lunghezza della parte dell'oggetto richiesta è uguale a 0 e la parte dell'oggetto non è un identificatore di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valido. Se la lunghezza del nome dell'oggetto è pari a 0, il nome completo risulta non valido.  
  
## <a name="examples"></a>Esempi

 Nell'esempio seguente viene utilizzato `PARSENAME` per restituire informazioni sulla tabella `Person` nel database `AdventureWorks2012`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT PARSENAME('AdventureWorksPDW2012.dbo.DimCustomer', 1) AS 'Object Name';  
SELECT PARSENAME('AdventureWorksPDW2012.dbo.DimCustomer', 2) AS 'Schema Name';  
SELECT PARSENAME('AdventureWorksPDW2012.dbo.DimCustomer', 3) AS 'Database Name';  
SELECT PARSENAME('AdventureWorksPDW2012.dbo.DimCustomer', 4) AS 'Server Name';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
```
Object Name
------------------------------
DimCustomer

(1 row(s) affected)

Schema Name
------------------------------
dbo

(1 row(s) affected)

Database Name
------------------------------
AdventureWorksPDW2012

(1 row(s) affected)

Server Name
------------------------------
(null)

(1 row(s) affected)
```
  
## <a name="see-also"></a>Vedere anche

- [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)  
- [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
- [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
- [Funzioni di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)  
  
  

