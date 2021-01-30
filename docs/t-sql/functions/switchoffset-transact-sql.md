---
description: SWITCHOFFSET (Transact-SQL)
title: SWITCHOFFSET (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/02/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SWITCHTZ
- SWITCHTZ_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- functions [SQL Server], time
- functions [SQL Server], date and time
- SWITCHOFFSET function [SQL Server]
- time [SQL Server], functions
- date and time [SQL Server], SWITCHOFFSET
- time zones [SQL Server]
ms.assetid: 32a48e36-0aa4-4260-9fe9-cae9197d16c5
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3b41bf1941bb12bf7560563d0b00fad7b88a75f8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208845"
---
# <a name="switchoffset-transact-sql"></a>SWITCHOFFSET (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce un valore **datetimeoffset** che è stato convertito dalla differenza di fuso orario archiviata a una nuova differenza di fuso orario specificata.  
  
 Per una panoramica di tutti i tipi di dati e delle funzioni di data e ora [!INCLUDE[tsql](../../includes/tsql-md.md)], vedere [Funzioni e tipi di dati di data e ora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
SWITCHOFFSET ( DATETIMEOFFSET, time_zone )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *DATETIMEOFFSET*  
 Espressione che può essere risolta in un valore di tipo **datetimeoffset(n)**.  
  
 *time_zone*  
 Stringa di caratteri nel formato [+|-]TZH:TZM o intero con segno (relativo ai minuti) che rappresenta la differenza di fuso orario. Deve essere sensibile all'ora legale e adattato ad essa.  
  
## <a name="return-type"></a>Tipo restituito  
 **datetimeoffset** con la precisione frazionaria dell'argomento *DATETIMEOFFSET*.  
  
## <a name="remarks"></a>Osservazioni  
 Usare SWITCHOFFSET per selezionare un valore **datetimeoffset** in una differenza fuso orario diversa dalla differenza fuso orario che è stata archiviata originalmente. SWITCHOFFSET non aggiorna il valore *time_zone* archiviato.  
  
 È possibile usare SWITCHOFFSET per aggiornare una colonna **datetimeoffset**.  
  
 L'utilizzo di SWITCHOFFSET con la funzione GETDATE() può determinare un'esecuzione lenta della query. Questo perché Query Optimizer non è in grado di ottenere stime relative alla cardinalità precise per il valore di data e ora. Per risolvere il problema, utilizzare l'hint per la query OPTION (RECOMPILE) per forzare la ricompilazione del piano di query da parte di Query Optimizer la volta successiva che viene eseguita la stessa query. A quel punto Query Optimizer disporrà di stime precise sulla cardinalità e offrirà un piano di query più efficace. Per altre informazioni sull'hint per la query RECOMPILE, vedere [Hint per la query &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
```sql
DECLARE @dt datetimeoffset = switchoffset (CONVERT(datetimeoffset, GETDATE()), '-04:00');   
SELECT * FROM t    
WHERE c1 > @dt OPTION (RECOMPILE);  
```  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene utilizzato `SWITCHOFFSET` per visualizzare una differenza di fuso orario diversa dal valore archiviato nel database.  
  
```sql  
CREATE TABLE dbo.test   
    (  
    ColDatetimeoffset datetimeoffset  
    );  
GO  
INSERT INTO dbo.test   
VALUES ('1998-09-20 7:45:50.71345 -5:00');  
GO  
SELECT SWITCHOFFSET (ColDatetimeoffset, '-08:00')   
FROM dbo.test;  
GO  
--Returns: 1998-09-20 04:45:50.7134500 -08:00  
SELECT ColDatetimeoffset  
FROM dbo.test;  
--Returns: 1998-09-20 07:45:50.7134500 -05:00  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [AT TIME ZONE &#40;Transact-SQL&#41;](../../t-sql/queries/at-time-zone-transact-sql.md)  
  
  


