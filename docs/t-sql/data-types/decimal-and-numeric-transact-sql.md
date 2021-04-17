---
title: decimal e numeric (Transact-SQL) | Microsoft Docs
description: Informazioni di riferimento Transact-SQL per i tipi di dati decimal e numeric. Decimal e numeric sono sinonimi dei tipi di dati numerici con precisione e scala fisse.
ms.date: 09/10/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- decimal
- decimal_TSQL
- numeric
- numeric_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- decimal data type
- decimal data type, about decimal data type
- numeric data type
- numeric data type, about numeric data type
ms.assetid: 9d862a90-e6b7-4692-8605-92358dccccdf
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3435df360f80a2971d35848ade603d69094fd442
ms.sourcegitcommit: a177a1e17200400a70f1d61b737481c83249e9a3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2021
ms.locfileid: "107584060"
---
# <a name="decimal-and-numeric-transact-sql"></a>decimal e numeric (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Tipi di dati numerici con precisione e scala fisse. Decimal e numeric sono sinonimi e possono essere usati in modo intercambiabile.
  
## <a name="arguments"></a>Argomenti
**decimal**[ **(** _p_[ **,** _s_] **)** ] e **numeric**[ **(** _p_[ **,** _s_] **)** ]  
Numeri con precisione e scala fisse. Se viene utilizzata la precisione massima, i valori validi sono compresi nell'intervallo da - 10^38 +1 a 10^38 - 1. I sinonimi ISO per **decimal** sono **dec** e **dec(** _p_, _s_ **)** . Dal punto di vista funzionale, **numeric** è identico a **decimal**.
  
p (precisione)  
Numero massimo totale di cifre decimali da archiviare. Include le cifre sia a destra sia a sinistra del separatore decimale. La precisione deve essere un valore compreso tra 1 e la precisione massima di 38. La precisione predefinita è 18.
  
> [!NOTE]  
>  Informatica supporta solo 16 cifre significative, indipendentemente dalla precisione e dalla scala specificate.  

*s* (scala)  
Numero massimo di cifre decimali che da archiviare a destra del separatore decimale. Questo numero viene sottratto da *p* per determinare il numero massimo di cifre a sinistra del separatore decimale. La scala deve essere un valore compreso tra 0 e *p* e può essere specificato solo se viene specificata la precisione. La scala predefinita è 0. Di conseguenza, 0 <= *s* \<= *p*. Le dimensioni massime di archiviazione variano a seconda della precisione.
  
|Precision|Byte per l'archiviazione|  
|---|---|
|1 - 9|5|  
|10-19|9|  
|20-28|13|  
|29-38|17|  
  
> [!NOTE]  
>  Informatica (connesso tramite il connettore Informatica di SQL Server PDW) supporta solo 16 cifre significative, indipendentemente dalla precisione e dalla scala specificate.  
  
## <a name="converting-decimal-and-numeric-data"></a>Conversione dei dati di tipo decimal e numeric
Per i tipi di dati **decimal** e **numeric**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considera ogni combinazione di precisione e scala un tipo di dati diverso. **decimal(5,5)** e **decimal(5,0)** sono ad esempio considerati tipi di dati diversi.
  
Nelle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] una costante con separatore decimale viene convertita automaticamente in un valore di tipo **numeric** in base ai valori di precisione e scala minimi necessari. Ad esempio, la costante 12.345 viene convertita in un valore **numeric** con precisione 5 e scala 3.
  
La conversione da **decimal** o **numeric** in **float** o **real** può determinare una perdita di precisione. La conversione da **int**, **smallint**, **tinyint**, **float**, **real**, **money** o **smallmoney** in **decimal** o **numeric** può determinare un overflow.
  
Per impostazione predefinita, quando si converte un numero in un valore **decimal** o **numeric** con precisione e scala inferiori, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene applicato l'arrotondamento. Se invece l'opzione SET ARITHABORT è impostata su ON, in caso di overflow [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] segnala un errore. La diminuzione di precisione e scala nelle operazioni di conversione non è sufficiente per generare un errore.
  
Prima di [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)], la conversione dei valori **float** in **decimal** o **numeric** è limitata ai soli valori con precisione a 17 cifre. Qualsiasi valore **float** minore di 5E-18 (se impostato usando la notazione scientifica di 5E-18 o la notazione decimale di 0.0000000000000000050000000000000005) viene arrotondato per difetto a 0. Questa non è più una restrizione a partire da [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)].
  
## <a name="examples"></a>Esempi  
Nell'esempio seguente viene creata una tabella usando i tipi di dati **decimal** e **numeric**.  I valori vengono inseriti in ogni colonna. I risultati vengono restituiti usando un'istruzione SELECT.
  
```sql
CREATE TABLE dbo.MyTable  
(  
  MyDecimalColumn DECIMAL(5,2)  
,MyNumericColumn NUMERIC(10,5)
  
);  
  
GO  
INSERT INTO dbo.MyTable VALUES (123, 12345.12);  
GO  
SELECT MyDecimalColumn, MyNumericColumn  
FROM dbo.MyTable;  
  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
MyDecimalColumn                         MyNumericColumn  
--------------------------------------- ---------------------------------------  
123.00                                  12345.12000  
  
(1 row(s) affected)  
  
```  
  
## <a name="see-also"></a>Vedere anche
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
[sys.types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)
  
