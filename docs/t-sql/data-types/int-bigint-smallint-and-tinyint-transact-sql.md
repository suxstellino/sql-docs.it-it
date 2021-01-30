---
title: int, bigint, smallint e tinyint (Transact-SQL) | Microsoft Docs
description: Informazioni di riferimento Transact-SQL per i tipi di dati int, bigint, smallint e tinyint. Questi tipi di dati vengono usati per rappresentare dati integer.
ms.date: 09/08/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- bigint_TSQL
- smallint
- bigint
- smallint_TSQL
- tinyint_TSQL
- int_TSQL
- int
- tinyint
dev_langs:
- TSQL
helpviewer_keywords:
- exact numeric data [SQL Server]
- numeric data
- tinyint data type
- int data type
- smallint data type
ms.assetid: 9bda5b0b-2380-4931-a1c8-f362fdefa99b
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 23242d075a453f244601ed573ed779d160d810bd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165134"
---
# <a name="int-bigint-smallint-and-tinyint-transact-sql"></a>int, bigint, smallint e tinyint (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Tipi di dati numerici esatti che utilizzano dati integer. Per risparmiare spazio nel database, usare il tipo di dati più piccolo che può contenere in modo affidabile tutti i valori possibili. Ad esempio, tinyint sarebbe sufficiente per l'età di una persona perché nessuno raggiunge un'età maggiore di 255 anni. Ma tinyint non sarebbe sufficiente per la durata di un edificio perché un edificio può avere più di 255 anni.
  
|Tipo di dati|Range|Archiviazione|  
|---|---|---|
|**bigint**|da -2^63 (-9.223.372.036.854.775.808) a 2^63-1 (9.223.372.036.854.775.807)|8 byte|  
|**int**|da -2^31 (-2.147.483.648) a 2^31-1 (2.147.483.647)|4 byte|  
|**smallint**|da -2^15 (-32.768) a 2^15-1 (32.767)|2 byte|  
|**tinyint**|da 0 a 255|1 byte|  
  
## <a name="remarks"></a>Osservazioni  
Il tipo di dati **int** è il tipo di dati integer primario in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il tipo di dati **bigint** è stato progettato per essere utilizzato quando i valori interi potrebbero non rientrare nell'intervallo supportato dal tipo di dati **int**.
  
Nell'ordine di precedenza dei tipi di dati **bigint** è compreso tra i tipi di dati **smallmoney** e **int**.
  
Le funzioni restituiscono **bigint** solo se l'espressione per il parametro è un tipo di dati **bigint**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non promuove automaticamente altri tipi di dati Integer (**tinyint**, **smallint** e **int**) to **bigint**.
  
> [!CAUTION]  
>  Quando si utilizzano gli operatori aritmetici +, -, \*, / o % per eseguire conversioni implicite o esplicite di costanti di tipo **int**, **smallint**, **tinyint**, o **bigint** per tipi di dati **mobili**, **reali**, **decimali** o **numerici**, le regole applicate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per calcolare il tipo di dati e la precisione dei risultati delle espressioni variano a seconda del fatto che per la query sia attivata o meno la parametrizzazione automatica.  
>   
>  Per questo motivo, in alcuni casi espressioni simili nelle query possono dare risultati diversi. Se per una query non è attivata la parametrizzazione automatica, il valore costante viene prima convertito nel tipo **numerico**, la cui precisione è sufficiente a contenere il valore della costante, quindi viene convertito nel tipo di dati specificato. Il valore costante 1, ad esempio, viene convertito in **numerico (1, 0)** e il valore costante 250 viene convertito in **numerico (3, 0)** .  
>   
>  Se per una query è attivata la parametrizzazione automatica, il valore costante viene sempre convertito in **numerico (10, 0)** prima di essere convertito nel tipo di dati finale. Se viene utilizzato l'operatore /, sia la precisione del tipo di risultati che il valore del risultato di query simili possono essere diversi. Ad esempio, il valore del risultato di una query con parametrizzazione automatica che include l'espressione `SELECT CAST (1.0 / 7 AS float)` è diverso dal valore del risultato della stessa query ma senza parametrizzazione automatica, perché i risultati della query con parametrizzazione automatica vengono troncati per rientrare nel tipo di dati **numerico (10, 0)** .  
  
## <a name="converting-integer-data"></a>Conversione di dati di tipo integer
Quando i numeri interi vengono convertiti implicitamente in un tipo di dati carattere, se il numero intero è troppo grande per adattarsi al campo del carattere, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] immette il carattere ASCII 42, l'asterisco (*).
  
Le costanti integer maggiori di 2.147.483.647 vengono convertite nel tipo di dati **decimale**, non nel tipo di dati **bigint**. Nell'esempio seguente viene illustrata la modifica del tipo di dati del risultato da **int** a **decimale** quando il valore soglia viene superato.
  
```sql
SELECT 2147483647 / 2 AS Result1, 2147483649 / 2 AS Result2 ;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
Result1      Result2  
1073741823   1073741824.500000  
```  
  
## <a name="examples"></a>Esempi  
Nell'esempio seguente viene creata una tabella utilizzando i tipi di dati **bigint**, **int**, **smallint** e **tinyint**. I valori vengono inseriti in ogni colonna e restituiti nell'istruzione SELECT.
  
```sql
CREATE TABLE dbo.MyTable  
(  
  MyBigIntColumn BIGINT  
,MyIntColumn  INT
,MySmallIntColumn SMALLINT
,MyTinyIntColumn TINYINT
);  
  
GO  
  
INSERT INTO dbo.MyTable VALUES (9223372036854775807, 2147483647,32767,255);  
 GO  
SELECT MyBigIntColumn, MyIntColumn, MySmallIntColumn, MyTinyIntColumn  
FROM dbo.MyTable;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
MyBigIntColumn       MyIntColumn MySmallIntColumn MyTinyIntColumn  
-------------------- ----------- ---------------- ---------------  
9223372036854775807  2147483647  32767            255  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Vedere anche
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
[sys.types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)
  
  
